# Portable Warning Gates

Merged !10318 first gates a warning by GCC version. In merged follow-up !10329, John Thacker points out that Intel front ends can advertise GNU compatibility and corrects the Clang identity macro to `__clang__`. The accepted helper distinguishes the actual front end before enabling GCC-specific warning controls.

**Portability rule:** a compatibility macro indicates supported language behavior, not necessarily compiler identity. Before applying implementation-specific warning controls, identify the actual front end as well as the claimed compatibility/version.
