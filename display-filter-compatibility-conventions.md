# Wireshark Display-Filter Compatibility Conventions

This file records durable conventions for user-visible display-filter field names. Current upstream source remains authoritative.

## Treat registered field abbreviations as a compatibility surface

Display-filter abbreviations are not merely internal labels. Users embed them in saved filters, profiles, command lines, scripts, documentation, and tooling, so changing an existing abbreviation can break workflows even when a replacement name is more accurate.

Merged master MR 5809 provides direct review evidence. Roland Knall objected to renaming existing PROFINET fields such as `pn_io.submodule_state.qualified_info` because users may already depend on those names in filters and profiles. His preferred migration was to retain the old field, mark it deprecated with expert information, and remove it only through an intentional deprecation path. He also identified an exception: a rename may be justified when the old field was effectively unused or its semantic classification was actually wrong. The merged change accepted semantic renames in that context, so this is not an absolute ban on renaming; compatibility must be reviewed explicitly rather than treated as cosmetic cleanup.

Merged master MR 5772 independently reinforces the same restraint. Alexis La Goutte disliked existing capitalization in CFM field/filter names, but Jaap Keuter chose to preserve the current naming while adding the new PDU and defer broader cleanup until a dedicated overhaul.

**Compatibility rule:** before renaming a registered field abbreviation, assume external users may depend on the old spelling. Prefer an alias or deliberate deprecation path when practical. If the old name is materially incorrect and a rename is still warranted, document the compatibility tradeoff explicitly.

**Review rule:** do not fold broad display-filter spelling or capitalization cleanup into an unrelated protocol feature merely for style consistency.

**Confidence:** High. The compatibility concern is explicit maintainer review in a merged MR and is independently corroborated by a merged decision to defer naming cleanup.
