# Wireshark Header Language-Linkage Conventions

## Headers own their C/C++ linkage contract

Merged Guy Harris-authored MRs !2425–!2455 establish that shared headers should not rely on callers wrapping the entire include in a C-linkage block. Included library/project headers stay outside that block; the declarations that require C linkage are wrapped by the header itself. The no-libpcap follow-up also exposed why feature guards must keep opening and closing linkage scopes balanced in every supported configuration.

**Implementation rule:** make a header self-contained for both C and C++ consumers. Do not put arbitrary included headers inside a broad language-linkage wrapper, and do not let optional-feature guards produce structurally different linkage scopes.

**Confidence:** Extremely high. Repeated merged master and stable changes authored by Guy Harris, including a concrete feature-disabled compile fix.
