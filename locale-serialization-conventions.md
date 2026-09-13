# Locale and Serialization Conventions

This file records durable rules for text serialization and parsing whose grammar must remain stable across user locales. Current upstream behavior remains authoritative.

## Machine-readable numeric serialization must not depend on the ambient locale

A serializer that emits text for a language parser or other machine-readable grammar must use the numeric syntax that grammar defines, even when the process locale uses another decimal separator. Parsing a single number successfully under the locale does not prove that a complete serialized expression will be accepted by the target lexer.

Merged master MR !21663, authored by John Thacker and approved by Anders Broman, fixes Wireshark's Lua test-table serialization. Lua `tonumber()` can accept the current locale's decimal separator, but Lua's lexer used by `load()` requires a period in numeric literals. Under a German locale the serializer could therefore emit comma-decimal numbers that made the complete table impossible to load. The accepted change temporarily selects the C numeric locale while serializing, restores the previous locale afterward, and adds a test that explicitly runs under `de_DE.utf-8`.

**Serialization rule:** use a canonical, grammar-defined numeric representation for machine-readable output rather than inheriting the user's numeric locale. Add regression coverage using a locale whose decimal separator differs from the canonical format so locale assumptions cannot remain invisible.

**Confidence:** Very high. Merged master fix by John Thacker with a targeted non-C-locale regression test.