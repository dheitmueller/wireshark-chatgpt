# Wireshark Platform-Conditional Testing Conventions

This file records durable testing/review conventions for platform-gated code. Current CI configuration and supported-platform policy remain authoritative.

## Functionally test the changed platform path; require build coverage on unaffected platforms

When a change is compiled or executed only on one platform, functional validation belongs on that platform. Other supported platforms still need enough coverage to prove that conditional compilation, shared declarations, and unchanged fallback paths continue to build, but contributors should not be expected to functionally exercise code that those platforms never execute.

During merged master MR !14326, which adds a DBus implementation of "Show in Folder" for Linux/DBus desktops, the contributor reported functional tests on Debian, confirmed the existing macOS path still worked, and noted that Windows was not tested. Guy Harris clarified the relevant review scope: because the changed code is not used on macOS or Windows, those systems merely need to show that the build is not broken; functional behavior needs to be exercised on the platform that actually uses the new code.

**Testing rule:** map tests to the conditional-compilation/runtime surface being changed. Exercise new platform-specific behavior on at least one representative target environment, and use CI/build checks on unaffected platforms to catch compile, link, include, and shared-interface regressions.

**Review rule:** do not demand redundant functional testing on platforms whose code path is unchanged and cannot reach the new implementation. Instead, verify the platform guard itself and ensure that the existing path still compiles there.

**Confidence:** Very high. The guidance is an explicit Guy Harris review comment on a merged platform-specific change and cleanly distinguishes runtime validation from cross-platform build compatibility.
