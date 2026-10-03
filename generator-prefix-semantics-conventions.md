# Generator Prefix-Semantics Conventions

## Use exact-prefix operations for exact-prefix intent

Merged master MR !60 fixes the nl80211 generator after Python `lstrip('nl80211_')` was used as though it removed one literal prefix. `lstrip` removes any leading characters belonging to the supplied character set, which corrupted several NAN-derived field abbreviations. The accepted change introduces exact-prefix removal in the generator and regenerates the affected dissector output.

**Generator rule:** choose string operations whose semantics match the transformation exactly. Prefix removal should first test the literal prefix and remove that precise substring; character-set stripping is not an equivalent shortcut.

**Source-of-truth rule:** when a generated identifier is wrong because of generator logic, fix the generator and regenerate the checked-in output rather than hand-editing only the derived file.

Merged !81 independently reinforces source/output synchronization by changing both the NGAP ASN.1 template and generated dissector for the reserved field type correction.

**Evidence weight:** Very high. Both examples are merged master changes.
