# Wiretap Format Dispatch Conventions

## Give distinct related formats distinct opener identities

Merged master MR !2762, authored by Guy Harris, adds support for CommView NCFX while retaining NCF. The accepted integration exposes separate NCF and NCFX opener entry points and separate file-extension identities.

Rule: related formats with different recognition or parsing contracts should have explicit format-specific opener identities, even when they share internal helpers.

Confidence: extremely high. Merged master Wiretap architecture change authored by Guy Harris.
