# Wireshark Scripting Numeric Conversion Conventions

This file records durable conventions for numeric conversions at Wireshark scripting-language boundaries. Current upstream source remains authoritative.

## Preserve the semantic signedness and width of the API domain

Do not choose a Lua conversion helper merely because the destination C expression happens to use `int`, `unsigned`, or `lua_Integer` on the current build. The conversion at the WSLua boundary must match the semantic domain of the API value, including its signedness and full valid range.

Merged master MR !15585, with merged release-4.2/4.0/3.6 counterparts !15590, !15591, and !15592, changes `DissectorTable` port handling from `luaL_checkinteger()` plus an implicit C conversion to `wslua_checkguint32()`. The dissector-table selector is a `uint32_t` domain; routing values through signed `Lua_Integer` can reject or corrupt values above `INT32_MAX` when Lua uses 32-bit integers. All four accepted variants apply the same semantic fix.

Merged master MR !15571, authored and merged by John Thacker, documents why signed and unsigned Lua conversions are not interchangeable. On older 32-bit Windows Lua builds, signed and unsigned conversion paths can use different machine-width behavior; converting a negative floating-point value directly to an unsigned C type is undefined and can differ between ARM and x86; and 64-bit values routed through `double` can lose precision. The same MR emphasizes that `Lua_Integer` width is a Lua configuration property, not a synonym for the platform's native integer width.

**Implementation rule:** use the WSLua helper whose accepted range and signedness match the public/API value. Avoid signed intermediates for unsigned protocol domains, and avoid floating-point intermediates for integer domains whose complete range cannot be represented exactly. Treat `sizeof(lua_Integer)` and Lua-version behavior as explicit compatibility inputs rather than assuming they track the host ABI.

## Numeric representation follows meaning, not a blanket modernization rule

Changing older Lua-number code to integer APIs is correct only when the exposed value is semantically integral. Merged master MR !15583 and release-4.2 MR !15584 revert a `lua_pushinteger()` conversion for a file-read value because the value can legitimately be fractional and therefore belongs in `lua_pushnumber()`. Review discussion on the stable branch also called out the need to consider the Lua versions and 32-bit configurations actually supported by that branch rather than applying master-side numeric cleanup mechanically.

**Review rule:** for each scripting conversion, ask what values the API is required to preserve, not which Lua primitive looks newest or most convenient. Audit negative values, values above signed maxima, 32-bit Lua builds, 64-bit integer precision, and genuinely fractional values. A conversion cleanup is incomplete until these boundary cases remain representable and portable.

**Confidence:** Extremely high. The evidence is a John Thacker-authored master series with three maintained-branch counterparts plus explicit portability rationale and a separate accepted revert where integer conversion was semantically wrong.
