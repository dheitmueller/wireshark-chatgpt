# Tooling Portability Conventions

## Match shell syntax and static-analysis inputs to their real execution environment

Merged master MR !2844, authored by Guy Harris, fixes `tools/validate-clang-check.sh` after an Ubuntu job reported that `[[ ... ]]` was unavailable. The script is expected to run with portable shell semantics, so the accepted change uses `test`/ordinary `if` constructs instead. The same MR excludes `extcap/etl.c` from Unix clang-check because that translation unit is only built on Windows. Merged follow-up !2853 immediately fixes the basename extraction used by those exclusions.

**Tooling rule:** scripts must stay within the language promised by their interpreter/shebang. Do not rely on Bash-only syntax in a `/bin/sh`-portable path.

**Analysis rule:** static-analysis input selection must follow the build matrix. A source file that only compiles under another platform's headers/toolchain is not a valid host-side analysis target merely because it changed in the commit.

**Validation rule:** run the tooling path itself after changing its file-selection logic; tool fixes can contain ordinary argument/variable mistakes just like production code.

**Confidence:** extremely high. Merged master tooling fixes authored by Guy Harris.
