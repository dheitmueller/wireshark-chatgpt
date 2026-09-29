# Formatted-I/O Portability Conventions

MR !5498 shows that fixed-width integer format macros are split by operation: the printing family belongs with formatted output and the scanning family belongs with formatted input. Stig Bjørlykke caught the wrong family in a scanning call and João Valverde corrected it before merge.

MRs !5497 and !5503 show that a broad formatting API migration must also compile supported platform-conditional paths. A macOS-only branch exposed a missing declaration that common builds did not catch.

**Rules:** review formatted input and output call sites separately during mechanical migrations, and compile representative supported platform configurations after broad API/header changes.

**Confidence:** Very high; all three are merged master changes and !5498 includes direct maintainer correction.
