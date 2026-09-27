# Dynamic Filter-Field Conventions

## Base filter identity on stable semantics and keep registration aligned with availability

Merged master MR !10513, authored and merged by John Thacker after extensive Stig Bjørlykke review, makes generated packet-list column fields filterable. Review rejected using user-visible column titles as field identity because titles can be localized, renamed, duplicated, or contain unsuitable characters. The accepted direction derives names from stable column-format identity and dynamically registers only fields for column types that actually exist in preferences.

The discussion also treats historical `_ws.col.*` spellings as script-visible compatibility surface and avoids manufacturing generic string fields for custom columns when their underlying typed field expressions are more semantically accurate.

**Implementation rule:** derive display-filter identifiers from stable semantic/programmatic identity, not localized or user-editable labels. When field existence depends on runtime configuration, keep registration and actual value availability synchronized. Prefer the real typed field when a generic string representation would weaken filter semantics.

**Compatibility rule:** established filter names are user/script API. Renames need deliberate compatibility handling or migration documentation rather than being treated as presentation-only changes.

**Confidence:** Very high. Merged master implementation with sustained reviewer discussion about identity, availability, semantics, and compatibility.

## Generated hidden fields can expose computed metadata without cluttering the tree

A value can be semantically useful for filtering or custom columns even when it is computed from configuration/state rather than represented by a literal packet field. Merged MR !7466 adds the configured Signal-PDU name as an `FT_STRING` item, marks it generated, and hides it from the normal tree so `signal_pdu.name` remains available to filters/columns without duplicating visible presentation.

**Implementation rule:** use a generated field for useful computed metadata that has no direct byte representation. If the value is primarily an automation/filter hook and would duplicate existing presentation, a hidden generated item can preserve a clean tree while still exposing the stable field API.

**Confidence:** High. Merged master dissector implementation.


## Keep dynamic registration proportional to configured features

Merged MR !7085 fixes a large Signal-PDU profile reload slowdown caused by registering aggregation fields for every configured signal even when aggregation was not enabled. The accepted design always registers the base/raw fields and registers each aggregation field family only when configuration requires it.

**Rule:** dynamic field registration should reflect actual runtime configuration. Avoid creating large optional field families that cannot be populated, and benchmark configuration reload paths at realistic scale.

**Confidence:** Very high. Merged performance fix with measured large-configuration impact.
