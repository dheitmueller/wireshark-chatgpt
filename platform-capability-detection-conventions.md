# Wireshark Platform Capability Detection Conventions

This file records durable conventions for build-time and runtime capability detection across operating-system SDK and library combinations. Current upstream source remains authoritative.

## Do not mistake SDK linkability for deployment-target availability

Build-time symbol probes answer whether the compiler/linker can resolve a symbol using the build environment. On platforms whose SDK describes symbols newer than the deployment target, that is not necessarily evidence that the symbol exists in the runtime library on every supported host. Weak-link annotations and source-level availability guards are useful only when the complete compiler, SDK, library metadata, and dynamic-linker behavior make them effective.

Merged master MR !12920, authored and merged by Guy Harris, fixes a concrete macOS failure exposed by Ventura with Xcode 15. The Sonoma SDK's libpcap `.tbd` advertises remote-capture APIs such as `pcap_open()`, so CMake's `check_function_exists()` succeeds even though older macOS system libpcap does not provide those symbols at runtime. The resulting executable could fail during dynamic linking before an `__builtin_available()` guard in application code had any opportunity to run. Because Sonoma's system implementations were stubs anyway, the accepted solution deliberately skips the feature probe when linking against Apple's system libpcap while retaining the probe for non-system libpcap builds. Merged release-4.2 backport !12923 carries the same policy.

**Implementation rule:** distinguish three questions: whether a symbol is declared by the SDK, whether the build linker can resolve it, and whether the deployed runtime library is guaranteed to provide it. A positive configure/link probe is insufficient when the SDK can expose symbols newer than the minimum supported OS. If runtime absence can prevent process startup, gate the feature at build/configuration time using a trustworthy library/platform identity or another mechanism that proves deployment availability.

**Review rule:** for platform capability checks, explicitly test the oldest supported runtime with a current SDK/toolchain, not only a matching build-and-run host. When relying on weak linking or source-level availability checks, verify that missing symbols can actually reach the guard rather than being resolved eagerly by the loader.

**Confidence:** Extremely high. The master fix was authored and merged by Guy Harris, documents the SDK/dyld failure mode in detail, and was immediately carried to the supported release branch.