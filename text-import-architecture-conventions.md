# Text-Import Architecture Conventions

Merged master MR !5484, authored by John Thacker, removes text2pcap's private scanner and routes the CLI through the shared text-import implementation used by Import from Hex Dump.

**Architecture rule:** when CLI and GUI surfaces expose the same import syntax and packet-construction semantics, keep parsing and conversion in one shared engine. Frontends should own option/UI collection, reporting, and lifecycle rather than duplicate the parser.

**Review rule:** compare frontend defaults and output contracts when unifying implementations so removing duplicate code does not silently change intentional frontend-specific behavior.

**Confidence:** Very high; merged master architecture refactor authored by John Thacker.
