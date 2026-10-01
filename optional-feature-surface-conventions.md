# Optional Feature Surface Conventions
Merged master MR 5975, authored by Guy Harris, repairs builds without ZLib. Dependency-only routines are compiled out, --compress-type=gzip is rejected with diagnostics listing only supported values, and the Qt gzip control is disabled. Release-3.6 MR 5976 carries the same fix.

Rule: a supported feature-disabled build must be coherent across implementation, command-line contract, and GUI affordances. Successful compilation alone is insufficient if the program still advertises or accepts an unavailable capability.

Confidence: extremely high; merged Guy Harris master fix plus stable backport.


## Gate only the code path that actually requires an optional dependency

Merged master !2894 temporarily wrapped the RTP Player entry point in `HAVE_LIBPCAP` and supplied a no-op implementation for no-libpcap builds. Guy Harris tested that supported configuration on macOS, showed that the broader RTP Player path was still expected to work and could crash with the stubbed entry point, and authored merged !2905 removing the guard.

**Implementation rule:** place an optional-dependency compile/runtime guard at the narrowest layer that actually uses the dependency. Do not disable an enclosing feature merely because its common execution path usually has that dependency available.

**Validation rule:** supported feature-disabled builds need runtime smoke coverage of the surviving UI/CLI actions, not just a successful compile. A dependency guard can make the binary link while still leaving a broken reachable path.

**Confidence:** Extremely high. The regression was reproduced by Guy Harris and the corrective master MR was authored by him.
