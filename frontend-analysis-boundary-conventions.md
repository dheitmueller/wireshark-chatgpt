# Frontend Analysis Boundary Conventions

## Keep reusable analysis code independent of frontend presentation

Analysis helpers that are shared by Qt, sharkd, command-line tools, or other frontends should expose data and failure state through an API rather than directly invoking frontend presentation facilities.

Merged master MR !10920 refactors UDP multicast statistics so sharkd can reuse the existing analysis path. Shared multicast code stops owning GUI alert behavior, returns tap-registration errors to its caller, exposes the packet-analysis callback for reuse, and leaves capture-file rescan behavior with the Qt dialog. sharkd can then consume the same analysis state and serialize it as JSON without depending on GUI behavior.

**Architecture rule:** reusable tap/statistics code should own analysis state and callbacks; the consuming frontend should own dialogs, text/JSON rendering, rescan policy, and other presentation-specific behavior.

**Review rule:** when extending existing UI-adjacent analysis to a headless frontend, audit the shared layer for direct UI dependencies and capture-file operations that belong to the caller. Move those effects outward rather than adding another frontend-specific branch to the shared implementation.

**Confidence:** High. Merged master refactor whose purpose and accepted dependency changes directly establish the frontend/backend boundary.
