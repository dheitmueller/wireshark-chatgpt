# Wireshark Public-Header Validation Conventions

This file records durable conventions for validating public/installable Wireshark headers. Current upstream source, packaging, and CI remain authoritative.

## Test shipped public headers as consumer-facing compilation units

A header that is installed for consumers must be usable from the source-language modes it claims to support without depending on incidental include order or on private build context. Wireshark itself can compile successfully while a shipped header is unusable by an external C consumer if another internal header happened to provide declarations first, or if C++-only constructs leak into a header intended for C.

Merged master MR !21482 adds a packaging-time check for shipped headers after a public include was found not to work from pure C. The accepted implementation generates inclusion of the installed header set on the fly rather than maintaining a parallel hand-written header-check source. Guy Harris was the designated reviewer and Anders Broman merged the change. The immediate c128 header corrections in !21445/!21495 provide the concrete failure mode that motivated making the consumer contract mechanically testable.

**Implementation rule:** mechanically include/preprocess installed public headers from a clean consumer context in each supported language mode, rather than relying on Wireshark's internal translation units to prove header usability. Generate the test input from the authoritative shipped-header list where practical so newly installed headers enter the check automatically.

**Confidence:** Very high. Merged master build/packaging validation with Guy Harris review assignment and Anders Broman merge, following an actual public-header C-compatibility failure.

## Include the headers that directly define macros and declarations you use

A widely included or public-facing header should be self-contained with respect to the symbols in its own declarations. Do not rely on some other header normally being included first and indirectly supplying an annotation, export macro, typedef, or declaration. Transitive include order is an implementation accident and can differ between translation units, tools, and external consumers.

Merged master MR !16504, authored and merged by Guy Harris, makes `tvbuff.h` directly include the headers that define `WS_DLL_PUBLIC` and the `WS_*` attribute macros it uses. The change removes dependence on surrounding include order and also lets standalone analysis tools such as cppcheck see the declarations in the same context as the compiler.

**Implementation rule:** if a header uses a macro, annotation, or type in its own interface, include the authoritative header that defines it unless the type is deliberately handled through a valid opaque forward declaration. Do not depend on unrelated callers or umbrella headers to establish that prerequisite first.

**Confidence:** Extremely high. Merged header-hygiene correction authored and merged by Guy Harris.
