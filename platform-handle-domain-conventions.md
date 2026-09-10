# Wireshark Platform Handle Domain Conventions

This file records durable platform-API type-domain guidance extracted from accepted upstream Wireshark review. Current platform APIs and upstream source remain authoritative.

## Preserve the semantic handle type required by the platform API

Opaque platform handles are not interchangeable merely because their current C representations are compatible. Erasing an `HMODULE` to `void *`, or passing it to an API that expects a different handle domain, removes type information that documents which acquisition and release operations are valid and can hide a real lifetime/API mismatch.

During merged MR !24787, Guy Harris identified the Windows module-handle path as suspicious: `LoadLibraryEx()` returns `HMODULE`, and he questioned treating that value as a generic `HANDLE`/`void *` and releasing it through an operation for a different handle class. He explicitly recommended retaining `HMODULE` even if the underlying typedef happens to have the same representation. The follow-up dependency update in merged !24803 included the Windows library-lifetime repair and successfully restored the newer libgcrypt.

**Implementation rule:** preserve the most specific platform-defined opaque type returned by an acquisition API and pair it with that API family's matching release operation. Do not generalize opaque handles to `void *` or interchange distinct handle typedefs solely because their machine representation currently matches.

**Confidence:** Extremely high. Direct review guidance from Guy Harris in a merged Windows-startup regression investigation, subsequently followed by a merged repair that allowed the dependency upgrade to proceed.
