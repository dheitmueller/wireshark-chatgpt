# Dynamic Filter-Field Conventions

## Base filter identity on stable semantics and keep registration aligned with availability

Merged master MR !10513, authored and merged by John Thacker after extensive Stig Bjørlykke review, makes generated packet-list column fields filterable. Review rejected using user-visible column titles as field identity because titles can be localized, renamed, duplicated, or contain unsuitable characters. The accepted direction derives names from stable column-format identity and dynamically registers only fields for column types that actually exist in preferences.

The discussion also treats historical `_ws.col.*` spellings as script-visible compatibility surface and avoids manufacturing generic string fields for custom columns when their underlying typed field expressions are more semantically accurate.

**Implementation rule:** derive display-filter identifiers from stable semantic/programmatic identity, not localized or user-editable labels. When field existence depends on runtime configuration, keep registration and actual value availability synchronized. Prefer the real typed field when a generic string representation would weaken filter semantics.

**Compatibility rule:** established filter names are user/script API. Renames need deliberate compatibility handling or migration documentation rather than being treated as presentation-only changes.

**Confidence:** Very high. Merged master implementation with sustained reviewer discussion about identity, availability, semantics, and compatibility.
