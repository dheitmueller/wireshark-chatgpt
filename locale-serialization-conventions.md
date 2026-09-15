# Locale and Serialization Conventions

This file records durable rules for text serialization and parsing whose grammar must remain stable across user locales. Current upstream behavior remains authoritative.

## Machine-readable numeric serialization must not depend on the ambient locale

A serializer that emits text for a language parser or other machine-readable grammar must use the numeric syntax that grammar defines, even when the process locale uses another decimal separator. Parsing a single number successfully under the locale does not prove that a complete serialized expression will be accepted by the target lexer.

Merged master MR !21663, authored by John Thacker and approved by Anders Broman, fixes Wireshark's Lua test-table serialization. Lua `tonumber()` can accept the current locale's decimal separator, but Lua's lexer used by `load()` requires a period in numeric literals. Under a German locale the serializer could therefore emit comma-decimal numbers that made the complete table impossible to load. The accepted change temporarily selects the C numeric locale while serializing, restores the previous locale afterward, and adds a test that explicitly runs under `de_DE.utf-8`.

**Serialization rule:** use a canonical, grammar-defined numeric representation for machine-readable output rather than inheriting the user's numeric locale. Add regression coverage using a locale whose decimal separator differs from the canonical format so locale assumptions cannot remain invisible.

**Confidence:** Very high. Merged master fix by John Thacker with a targeted non-C-locale regression test.

## Machine-readable exporters should request the machine representation from the field-value layer

Human display text and machine serialization are different contracts. When Wireshark's field-value layer already exposes a representation intended for a machine grammar, exporters should request that representation rather than starting from the human display representation and accumulating exporter-specific exceptions.

Merged master MR !19316, authored and merged by John Thacker, changes `-T ek` field formatting from `FTREPR_DISPLAY` to `FTREPR_JSON`, matching `-T json`. That lets the shared ftype representation logic own details such as ISO 8601 absolute-time formatting and removes a redundant EK-specific absolute-time special case. The accepted change is a useful architectural example: choose the representation according to the consumer contract, then centralize type semantics below the individual exporter.

Merged master MR !19329, also authored by John Thacker, adds a persistent machine-readable-output preference for conversation and endpoint taps and exposes the same choice in the GUI. In later discussion Guy Harris explicitly called out that output ideal for humans and output ideal for programs do not always coincide, and asked where else the distinction would be useful.

**Representation rule:** do not assume presentation-oriented strings are suitable serialization values. For JSON/EK or another machine-consumed grammar, use the representation mode defined for that grammar and keep field-type formatting semantics in the shared representation layer when possible. When a statistics feature legitimately supports both audiences, make the human-vs-machine choice explicit and apply it consistently across CLI and GUI surfaces that expose the same data.

**Confidence:** Very high. Independent merged master changes by John Thacker, with direct architectural reinforcement from Guy Harris.