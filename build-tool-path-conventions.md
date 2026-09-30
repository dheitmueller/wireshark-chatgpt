# Build Tool Path Conventions

Merged MR !7019 fixes Windows setup after archive extraction began using CMake. The parent CMake build passes its resolved `CMAKE_COMMAND` path into `win-setup.ps1` instead of assuming a `cmake` executable can be found through PATH.

**Rule:** when the parent build system has already resolved a required tool, pass that exact executable to helper scripts. A second PATH lookup can select a different version or fail in an otherwise valid configured build environment.

**Confidence:** High. Merged build-system fix by Gerald Combs.


## Keep a project-owned module path when dependency discovery mutates the shared search path

Merged master MR !5111, authored by Jörg Mayer, handles Qt 6 extending CMake's `CMAKE_MODULE_PATH`. Wireshark saves its own module directory separately as `WS_CMAKE_MODULE_PATH` and uses that stable project-local path when naming Wireshark-owned CMake modules such as `FindGLIB2.cmake`, `UseAsn2Wrs.cmake`, and `UseMakePluginReg.cmake`.

**Rule:** when a dependency's package/configuration logic can mutate a global search-path variable, do not rely on that mutable variable to identify project-owned build modules. Preserve the project's authoritative module directory separately and use it for references that must resolve to Wireshark's own file.

**Review rule:** major dependency migrations should audit shared CMake variables for side effects from package discovery, not only whether `find_package()` succeeds.

**Confidence:** High. Merged master build-system fix that isolates a concrete Qt 6 side effect without changing normal Qt 5 resolution.


## Use the output directory of the concrete target being invoked

Merged master MR !4062 removes a hand-built `WS_PROGRAM_PATH` and uses CMake's `$<TARGET_FILE_DIR:tshark>` generator expression. Merged !4074 then corrects the test runner to use `$<TARGET_FILE_DIR:wmem_test>`, because the tshark directory represented only some executable layouts on macOS.

**Rule:** derive an executable path from the exact CMake target needed by the command. Do not reconstruct platform/configuration layouts manually, and do not assume a different executable target necessarily lands in the same output directory.

**Confidence:** Very high. Two merged build-system fixes authored by Gerald Combs.
