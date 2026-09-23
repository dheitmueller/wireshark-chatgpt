# Wireshark Platform Capability Detection Conventions

This file records durable conventions for build-time and runtime capability detection across operating-system SDK and library combinations. Current upstream source remains authoritative.

## Do not mistake SDK linkability for deployment-target availability

Build-time symbol probes answer whether the compiler/linker can resolve a symbol using the build environment. On platforms whose SDK describes symbols newer than the deployment target, that is not necessarily evidence that the symbol exists in the runtime library on every supported host. Weak-link annotations and source-level availability guards are useful only when the complete compiler, SDK, library metadata, and dynamic-linker behavior make them effective.

Merged master MR !12920, authored and merged by Guy Harris, fixes a concrete macOS failure exposed by Ventura with Xcode 15. The Sonoma SDK's libpcap `.tbd` advertises remote-capture APIs such as `pcap_open()`, so CMake's `check_function_exists()` succeeds even though older macOS system libpcap does not provide those symbols at runtime. The resulting executable could fail during dynamic linking before an `__builtin_available()` guard in application code had any opportunity to run. Because Sonoma's system implementations were stubs anyway, the accepted solution deliberately skips the feature probe when linking against Apple's system libpcap while retaining the probe for non-system libpcap builds. Merged release-4.2 backport !12923 carries the same policy.

**Implementation rule:** distinguish three questions: whether a symbol is declared by the SDK, whether the build linker can resolve it, and whether the deployed runtime library is guaranteed to provide it. A positive configure/link probe is insufficient when the SDK can expose symbols newer than the minimum supported OS. If runtime absence can prevent process startup, gate the feature at build/configuration time using a trustworthy library/platform identity or another mechanism that proves deployment availability.

**Review rule:** for platform capability checks, explicitly test the oldest supported runtime with a current SDK/toolchain, not only a matching build-and-run host. When relying on weak linking or source-level availability checks, verify that missing symbols can actually reach the guard rather than being resolved eagerly by the loader.

**Confidence:** Extremely high. The master fix was authored and merged by Guy Harris, documents the SDK/dyld failure mode in detail, and was immediately carried to the supported release branch.

## Runtime policy can revoke a capability that the library version appears to provide

Compile-time headers and library-version checks are also insufficient when a dependency has runtime modes that disable otherwise-present functionality. Treat the effective runtime capability as distinct from the library's nominal API surface, and make an optional feature degrade locally when the runtime environment denies that capability.

Merged master MR !12571, authored by John Thacker, handles libgcrypt FIPS mode for WireGuard. Wireshark's minimum libgcrypt version provides the required crypto APIs, but FIPS mode can make BLAKE2s and ChaCha20 unavailable at runtime. The accepted code records whether WireGuard decryption is actually supported, avoids creating or consuming key state when it is not, and reports the limitation with packet-tree expert information instead of reaching dissector assertions or emitting an unconditional startup warning even when no WireGuard traffic is present. Merged release-branch backports !12605 and !12606 carry the same behavior.

The same family also shows that changing a dependency's global runtime mode, when that is an intentional application policy, has an initialization-order contract. Merged release backports !12563 and !12564 call libgcrypt's `GCRYCTL_NO_FIPS_MODE` only when the library version provides it and do so before `gcry_check_version()`, because the mode must be selected before normal library initialization. That mitigation does not remove the need for feature-local fallback on older libraries where the mode cannot be disabled.

**Implementation rule:** distinguish API presence from effective runtime capability. If a policy/environment mode can revoke an algorithm or service, probe or handle that condition at runtime and keep unsupported optional functionality out of its normal state-mutating path. Prefer a feature-local unavailable result and precise expert/diagnostic information over assertions, crashes, or noisy process-wide warnings.

**Initialization rule:** when Wireshark intentionally changes a dependency's global mode, obey the dependency's required pre-initialization ordering and version-gate the control API. A global-mode override and a feature-local fallback solve different compatibility cases; do not assume one eliminates the other.

**Testing rule:** exercise the feature under the restrictive runtime mode as well as under the normal one. A successful ordinary build and test run does not demonstrate that the runtime capability remains available under policy modes such as FIPS.

**Confidence:** Very high. The master feature-local fallback was authored by John Thacker and merged, immediately followed by maintained-branch backports; the related libgcrypt initialization changes were likewise accepted across supported release branches.

## Exported symbols can still be nonfunctional compatibility stubs

A symbol that exists in the runtime library can still fail to provide the capability its API name suggests. Platform vendors may ship compatibility stubs so applications link successfully even though a feature is unavailable. Treat successful symbol discovery as evidence of API shape, not necessarily operational capability.

Merged master MR !12228, authored by Guy Harris, handles macOS 14 system libpcap routines for remote capture that are present but implemented as stubs returning `not supported`. CMake therefore finds the APIs, but they must not be used for ordinary local capture. The accepted change selects the remote-capture API only when the device name actually uses the `rpcap://` scheme, keeps local capture on `pcap_open_live()`, and translates the generic stub failure into the more actionable `Remote capture not supported`. Merged release backport !12229 carries the same behavior.

**Implementation rule:** distinguish symbol presence from operational support. When a capability has a semantic precondition that can be checked cheaply—such as an input identifying a remote-capture URL—use that precondition to select the specialized API instead of calling a nominally available routine speculatively. If runtime invocation can still report unsupported, propagate that as a capability result rather than as an unexplained generic failure.

**Diagnostic rule:** when a platform compatibility stub returns a generic error, translate it at the abstraction boundary into the feature the user was attempting to use. Preserve the underlying failure semantics, but make the unavailable capability clear enough to diagnose.

**Confidence:** Extremely high. The master fix was authored by Guy Harris and directly documents the Sonoma libpcap stub behavior; the release backport immediately preserved the same policy.