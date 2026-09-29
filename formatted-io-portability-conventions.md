# Formatted-I/O Portability Conventions

MR !5498 shows that fixed-width integer format macros are split by operation: the printing family belongs with formatted output and the scanning family belongs with formatted input. Stig Bjørlykke caught the wrong family in a scanning call and João Valverde corrected it before merge.

MRs !5497 and !5503 show that a broad formatting API migration must also compile supported platform-conditional paths. A macOS-only branch exposed a missing declaration that common builds did not catch.

**Rules:** review formatted input and output call sites separately during mechanical migrations, and compile representative supported platform configurations after broad API/header changes.

**Confidence:** Very high; all three are merged master changes and !5498 includes direct maintainer correction.


## Match the format macro to both the integer typedef and the formatting implementation

Equal-width integer types are not necessarily the same C type on every platform. A 64-bit GLib typedef can resolve to `long` where the corresponding C99 fixed-width format macro expects `long long`, producing varargs format diagnostics or undefined behavior.

Merged master MR !4564 fixes BPSEC/BPv7/COSE macOS builds by replacing C99 PRI macros at GLib-typed call sites with GLib's 64-bit format macros. Pascal Quantin pointed to Wireshark's documented GLib formatting convention. João Valverde added the important boundary: the entire call path must mesh—GLib formatting routines receiving `guint64` belong with GLib macros, while standard printf-family routines receiving `uint64_t` belong with C99 PRI macros.

**Portability rule:** choose the format specifier family from the actual argument typedef and the varargs formatting API, not merely from the nominal bit width. Do not assume `uint64_t`, `guint64`, `long`, and `long long` have identical underlying types across supported platforms.

**Validation rule:** compile format-heavy changes on representative macOS/Linux/Windows architectures because a type combination that is indistinguishable on one ABI can be diagnosed on another.

**Confidence:** Very high. Merged master portability fix with direct Pascal Quantin and João Valverde review.
