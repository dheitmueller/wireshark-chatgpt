# Wireshark Preference-Migration Conventions

This file records durable conventions for changes to persisted Wireshark preferences and their representations. Current upstream preference APIs and source remain authoritative.

## Replacing a persisted preference requires an upgrade path for existing users

Changing a preference from one persisted representation to another is a compatibility change, not just a registration cleanup. Existing preference files can contain meaningful user configuration that must not silently disappear merely because the new implementation uses a UAT, a differently named preference, or another storage form.

During review of merged master MR !24424, which replaces NTLMSSP's single `nt_password` string preference with a credentials UAT, Michael Mann explicitly required that the old preference be treated as obsolete only together with an upgrade path that carries an existing saved password into the new UAT. The review discussion also exposed an important implementation constraint: simply registering the old preference as obsolete causes Wireshark to ignore it while reading preferences, so migration must be designed deliberately rather than assuming obsolescence itself preserves the value.

**Implementation rule:** before removing or replacing a persisted preference, define how values from existing profiles are recognized and migrated to the new representation. Marking the old key obsolete is not by itself a migration strategy; verify that the upgrade mechanism can still access the legacy value at the point where conversion occurs.

**Confidence:** High. Direct maintainer review requirement on a merged master MR. The durable requirement is the compatibility/migration obligation; the exact migration API should be checked against current preference infrastructure when implementing a future change.


## A preference rename needs an old-key alias, not only a new registration name

Merged master MR !7812 modernizes TCP experimental-option terminology and renames the persisted preference accordingly. The accepted implementation explicitly recognizes the old preference key during preference loading and redirects it to the new preference, so existing profiles retain their prior effective setting.

**Implementation rule:** when a persisted preference is renamed without intentionally changing user configuration, keep an explicit old-name-to-new-name alias or migration path in the preference loader. Verify the old key still produces the intended new setting.

This is a concrete early example of the broader migration obligation described above: replacing or renaming a preference is a compatibility change, not merely a source-code naming cleanup.

**Confidence:** High. Merged master implementation with an explicit compatibility mapping and regression-oriented capture coverage in the same change.


## Match automatic/Decode As preference storage to the dispatch table's real cardinality

Merged master MRs !7611 and !7637, authored by John Thacker, establish that Decode As-backed port preferences should use a range even when the default consists of a single port. Decode As can bind a dissector to several values simultaneously; a scalar preference can represent only the last value and cannot faithfully round-trip the full binding set. With a range representation, an empty value also has a natural meaning: clear the set.

Merged master MR !7658 extends the compatibility side of the same migration. When an old explicit preference already has the same name as the automatic table preference, the explicit registration can be removed without losing the persisted key. When the old preference name differs, the accepted change adds an old-name-to-new-table mapping in `deprecated_port_prefs` so existing profiles continue to work. Because that migration helper owns the legacy key, the old preference does not also need to be registered as an obsolete preference.

**Representation rule:** choose preference storage that can represent every state the associated UI/Decode As mechanism can create. Do not back a multi-value dispatch configuration with a scalar merely because the default is one value.

**Migration rule:** when converting explicit port preferences to automatic table preferences, preserve old persisted names. Same-name conversions can rely on the retained key; differently named preferences need an explicit migration alias.

**Confidence:** Very high. Three merged master changes authored by John Thacker, with the cardinality and compatibility rationale stated directly.
