# Preference-Key Compatibility Conventions

## Persisted preference keys are compatibility identifiers

Merged master MR !87, authored and merged by Martin Mathieson, fixes user-visible spelling errors but deliberately leaves the misspelled preference key `st_sort_casesensitve` unchanged because renaming it would lose an existing saved setting.

**Implementation rule:** treat preference keys as part of the persisted configuration interface. Fix display labels/help text freely, but migrate or alias a persisted key if its name must change.

**Review rule:** spelling/cleanup passes must distinguish prose from machine/persistence identifiers. A typo in a compatibility-facing key can be safer to preserve than to "fix" without migration.

**Evidence weight:** Very high. The compatibility rationale is explicit in the merged master MR.
