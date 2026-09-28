# Wireshark Dependency Major-Version Gating Conventions

Merged master MR !5856, authored by Gerald Combs, adds Sparkle framework version discovery and requires Sparkle 1 exactly until the Sparkle 2 API is supported. The durable convention is that dependency discovery includes API-compatible version checks; a newly installed incompatible major is not interchangeable merely because headers and a library are present.
