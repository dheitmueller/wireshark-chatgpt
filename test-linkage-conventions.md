# Wireshark Test-Linkage Conventions

This file records durable build/linkage conventions for unit and internal tests. Current upstream CMake targets remain authoritative.

## Internal unit tests may link a static implementation target instead of widening the public ABI

A unit test that intentionally exercises internal routines should link against a target that contains those internal symbols rather than forcing those symbols into the shared library's exported interface or manually reconstructing all of the implementation's private link dependencies.

In merged master MR !24393, John Thacker identified that `wmem_test` uses symbols hidden from shared `libwsutil` and proposed linking the test against `wsutil_static`. Jaap Keuter confirmed the approach worked, and Guy Harris explicitly endorsed the model by noting the same pattern in libpcap, where some unit tests test internal rather than exported routines. The accepted MR also makes xxhash an implementation-private dependency of wsutil so plugins linking wsutil do not inherit a dependency they do not themselves use; release-4.6 backports !24404 and !24405 preserve the arrangement.

**Implementation rule:** keep implementation-only dependencies and symbols private to the library that owns them. If a true internal unit test needs non-exported code, link it to the appropriate static/internal target rather than expanding the public ABI or making normal consumers reproduce private dependencies.

**Confidence:** Extremely high. Merged master build change authored by John Thacker with explicit agreement from Jaap Keuter and high-authority corroboration from Guy Harris.