# Wireshark Platform Integer-Range Conventions

Merged MR !7902 fixes 32-bit Linux timestamp handling. Guy Harris pointed out that the proposed `WTAP_NSTIME_SECS_MAX` name sounded like a universal timestamp maximum, while the code actually needed the maximum positive seconds value that could be stored in a 32-bit representation given the platform's `time_t` width. The accepted constant was renamed `WTAP_NSTIME_32BIT_SECS_MAX`.

**Rule:** name range constants for the actual representation or protocol constraint being enforced, not for a broader type concept.

**Portability rule:** derive bounds from the width and signedness of the receiving representation rather than assuming the host type has one fixed width across supported platforms.

**Confidence:** Very high. Merged portability fix with direct Guy Harris review and approval.
