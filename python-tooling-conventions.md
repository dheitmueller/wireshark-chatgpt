# Wireshark Python Tooling Conventions

This file records durable conventions for invoking and implementing Python tooling in Wireshark automation. Current upstream scripts remain authoritative.

## Reinvoke Python helpers with the current interpreter

Automation that launches another Python script should normally invoke it through `sys.executable` instead of assuming the script is directly executable or that a compatible `python` appears first in `PATH`.

Merged MR !25025, authored and merged by John Thacker, fixes `tools/weekly-updates.py` on Windows by launching helper scripts with `sys.executable`. Python scripts are not necessarily directly executable on Windows, and using the current interpreter also ensures that child helpers run in the same Python environment on other platforms rather than accidentally selecting another installation from `PATH`.

**Implementation rule:** when one Python tool launches another Python tool as part of the same workflow, prefer `[sys.executable, script, ...]` (or the equivalent API shape) unless there is a deliberate reason to switch interpreters. Do not make executable-bit, shebang, file-association, or `PATH` behavior part of the implicit contract.

**Confidence:** Very high. Merged master portability fix authored and merged by John Thacker.

## Multiprocessing workers must not depend on fork-inherited initialization

Python project tools must work under multiprocessing start methods that create a fresh interpreter, not only under POSIX `fork`. Module globals referenced by worker functions therefore need a valid import-time definition or explicit worker initialization; defining them only inside `if __name__ == '__main__':` makes the worker contract platform-dependent.

Merged master MR !23460 fixes `tools/check_spelling.py` on Windows after spawned worker processes raised `NameError` for `wiki_db`. The variable had only been created by main-process initialization, which happens to be inherited under `fork` but is absent when Windows starts workers using `spawn`. Martin Mathieson reviewed and merged the correction. Related merged !23484 broadens another project check to run locally on Windows, reinforcing that these developer checks are expected to be cross-platform tools rather than Linux-CI-only scripts.

**Implementation rule:** when a Python checker uses `multiprocessing` or `ProcessPoolExecutor`, audit worker-visible module state under `spawn` semantics. Give referenced globals safe import-time definitions or initialize workers explicitly, and avoid relying on state that exists only because a forked child inherited the parent process image.

**Confidence:** Very high. Merged Windows portability fix with direct maintainer review and a concrete platform-specific failure.