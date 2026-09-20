# Wireshark Scripting Error Boundary Conventions

This file records durable conventions for containing scripting-language failures at the native application boundary. Current upstream WSLua behavior remains authoritative.

## Run script-triggerable registration and commit work inside a protected language boundary

Native code that invokes scripting-language operations capable of raising script errors must use the language runtime's protected-call mechanism when an error should be reported to the user rather than terminate Wireshark. A function being called from C does not make the script exception safe automatically.

Merged master MR !15611, authored and merged by John Thacker, fixes a WSLua crash when a field or expert item is registered twice. `Proto_commit()` can raise a Lua error, but it had been called directly from `wslua_init`; the error therefore reached `wslua_panic` and aborted the process. The accepted implementation calls it through `lua_pcall`, maps the Lua error classes to `report_failure()`, and improves the duplicate-registration diagnostic to include the field abbreviation. The same correction was accepted on maintained branches in !15616, !15617, and !15618.

**Implementation rule:** identify every native-to-script call that can raise through the scripting runtime. If failure is part of normal user/plugin error handling, invoke it through the runtime's protected-call facility and translate the result into Wireshark's reporting path. Reserve panic/abort behavior for genuinely unrecoverable runtime corruption, not malformed or conflicting plugin registrations.

**Review/testing rule:** exercise invalid plugin/script input that raises during registration and reload, not just during ordinary packet callbacks. Tests should verify that the error is reported with enough object identity to diagnose the script and that the Wireshark process remains alive.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker and subsequently backported across maintained branches.