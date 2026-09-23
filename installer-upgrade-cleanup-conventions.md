# Wireshark Installer Upgrade-Cleanup Conventions

This file records durable packaging/installer conventions for upgrading over older Wireshark installations. Current packaging source remains authoritative.

## Removing a shipped file can require retaining installer cleanup for old installations

Deleting a file from the current package does not guarantee that an upgrade-in-place removes copies installed by older releases. Installer scripts may need to keep explicit removal logic for obsolete files even after the corresponding current-install step disappears.

Merged master MR !11815, authored and merged by João Valverde, removes stale `init.lua` references and fixes packaging. Gerald Combs specifically cautioned against deleting NSIS cleanup entries merely because current releases no longer install the file: users can install a new version over an old one without uninstalling first, leaving historical files behind. His preferred pattern was to retain the removal entry and annotate it as a file installed by previous versions. João's follow-up also identified duplicated cleanup introduced by an earlier change, so the accepted result keeps the needed historical cleanup without redundant entries.

**Packaging rule:** when removing or renaming an installed file, audit upgrade-in-place behavior from supported/realistic older releases. Keep uninstall/cleanup rules for obsolete artifacts as long as those artifacts can remain on upgraded systems, and document why a seemingly unreferenced cleanup entry still exists.

**Review rule:** distinguish current package contents from upgrade state. A search showing that no current component installs a file is not sufficient justification to remove its cleanup path; verify what previous versions installed and what the installer does when upgrading without a prior uninstall.

**Confidence:** Very high. Merged master packaging fix with explicit Gerald Combs review describing the upgrade-in-place requirement and an accepted implementation that preserves historical cleanup semantics.
