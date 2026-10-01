# pcapng Identifier-Governance Conventions

## Do not invent values in standardized block or option namespaces

Merged master MR !2424, authored by Guy Harris, adds explicit warnings beside pcapng block and option dispatch tables that every identifier handled as a standardized code must actually appear in the pcapng specification. A Wireshark-private feature must not claim an unassigned value in a standards-governed namespace merely because the parser can recognize it.

**Implementation rule:** use pcapng's private/custom extension mechanisms for private formats. If a new block or option is intended to be standardized, obtain the assignment through the standards process before adding it to the core standardized-code tables.

**Architecture rule:** keep namespace governance separate from extension dispatch. Generic plugin/extension mechanisms may handle assigned or private extension identifiers, but they do not authorize squatting on standardized code points.

**Confidence:** Extremely high. Merged master guidance authored by Guy Harris and consistent with the notebook's later pcapng extension-registration architecture.
