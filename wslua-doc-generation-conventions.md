# WSLua Documentation-Generation Conventions

## Public Lua attributes must be accepted by the documentation generator

WSLua source annotations feed generated Developer's Guide API documentation. A public attribute addition is incomplete if its valid identifier spelling is outside the parser grammar used by the documentation generator.

Merged master MR !659 makes `pinfo.p2p_dir` mutable from Lua. During review, Christopher Maynard noticed that the attribute was missing from the generated Pinfo documentation. Peter Wu traced the omission to `docbook/make-wsluarm.pl`: the `WSLUA_ATTRIBUTE` parser allowed digits before the first underscore but not in the attribute-name portion after it. Merged follow-up MR !673 broadens that identifier grammar to accept digits there as well.

**API/documentation rule:** keep the generator grammar aligned with the actual public WSLua identifier grammar. If a legitimate identifier is skipped, fix the generator rather than hand-documenting one exceptional symbol.

**Review/testing rule:** after adding or changing a public WSLua attribute, regenerate or inspect the Developer's Guide output and confirm the symbol, access mode, and description are present. Include identifier edge cases such as embedded digits when changing generator parsing.

**Confidence:** Very high. The documentation omission was caught during review of merged !659, Peter Wu identified the root cause, and merged !673 implements the generator-level fix.
