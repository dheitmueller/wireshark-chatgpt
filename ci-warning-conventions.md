# Wireshark Compiler Warning and CI Conventions

Merged master MR !8853, authored by João Valverde, makes warning enablement an explicit project-policy choice rather than mechanically enabling every diagnostic a compiler offers. The accepted configuration separates ordinary warnings, actionable warnings that still need cleanup, and more pedantic diagnostics kept opt-in.

**Rule:** evaluate new warnings for signal quality, portability, generated-code impact, and cleanup cost before enabling them project-wide.

Merged master MR !8829 keeps the normal GCC build strict while configuring ASan, fuzzing, and Valgrind jobs so compiler warnings are not fatal there.

**CI rule:** warning strictness should match job purpose. Keep a strict build that catches warning regressions, while allowing runtime-analysis jobs to execute when an incidental warning does not invalidate the binary.

Merged master MR !8860, authored by John Thacker, fixes an older-Qt build by explicitly including QVector instead of relying on another Qt header to include it transitively.

**Portability rule:** directly include the public dependency header that defines a type or API used by the source. Transitive include graphs can change across supported dependency versions.
