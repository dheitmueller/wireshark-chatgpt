# Optional Feature Surface Conventions
Merged master MR 5975, authored by Guy Harris, repairs builds without ZLib. Dependency-only routines are compiled out, --compress-type=gzip is rejected with diagnostics listing only supported values, and the Qt gzip control is disabled. Release-3.6 MR 5976 carries the same fix.

Rule: a supported feature-disabled build must be coherent across implementation, command-line contract, and GUI affordances. Successful compilation alone is insufficient if the program still advertises or accepts an unavailable capability.

Confidence: extremely high; merged Guy Harris master fix plus stable backport.
