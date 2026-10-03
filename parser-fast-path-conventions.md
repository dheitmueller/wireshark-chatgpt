# Parser Fast-Path Conventions

## Keep failure sentinels outside the valid parsed-value domain

An optimized parser must not encode “invalid input” using a bit pattern that valid input can also produce. The fast path and the full parser must agree on the valid grammar and result domain.

Merged master MR !707 fixes Ethernet address parsing where the fast path tried to detect an invalid hexadecimal-nibble sentinel with a high-bit test. That test also rejected legitimate octets from 0x80 through 0xff, forcing valid input onto the slow path. Peter Wu helped refine the merged solution: compute in a wider integer domain so the invalid nibble remains distinguishable from all legal byte values, validate there, then narrow only after success.

The same review caught a grammar issue after adding support for both ':' and '-' separators. The accepted code records the separator selected by the first delimiter and requires later delimiters in the same address to match it, rather than accidentally accepting mixed forms.

**Implementation rule:** choose intermediate types and sentinel encodings so every legal parsed value remains representable without collision with failure state. Narrow to the destination type only after validation.

**Equivalence rule:** a fast path may support only a subset of the reference parser's grammar, but anything it accepts must have the same syntax and semantics as the reference path. When it adds an alternate syntax, enforce all token-level consistency rules rather than only the first delimiter.

**Testing rule:** test boundary values around any sentinel-related bit pattern, including the full high half of an unsigned byte domain, and malformed strings that mix otherwise valid separators.

**Confidence:** Very high. Merged master optimization with detailed Peter Wu review and an explicit example of the previously misclassified valid domain.
