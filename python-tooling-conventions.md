# Wireshark Python Tooling Conventions

This file records durable conventions for invoking Python tooling in Wireshark automation. Current upstream scripts remain authoritative.

## Reinvoke Python helpers with the current interpreter

Automation that launches another Python script should normally invoke it through `sys.executable` instead of assuming the script is directly executable or that a compatible `python` appears first in `PATH`.

Merged MR !25025, authored and merged by John Thacker, fixes `tools/weekly-updates.py` on Windows by launching helper scripts with `sys.executable`. Python scripts are not necessarily directly executable on Windows, and using the current interpreter also ensures that child helpers run in the same Python environment on other platforms rather than accidentally selecting another installation from `PATH`.

**Implementation rule:** when one Python tool launches another Python tool as part of the same workflow, prefer `[sys.executable, script, ...]` (or the equivalent API shape) unless there is a deliberate reason to switch interpreters. Do not make executable-bit, shebang, file-association, or `PATH` behavior part of the implicit contract.

**Confidence:** Very high. Merged master portability fix authored and merged by John Thacker.