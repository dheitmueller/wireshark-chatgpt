# Wireshark Field Display-Mode Conventions

This file records durable conventions for interpreting `header_field_info.display` and associated metadata. Current upstream source remains authoritative.

## Treat BASE_CUSTOM as a distinct semantic display mode

`BASE_CUSTOM` is not a modifier to combine with an ordinary numeric base. A custom field supplies formatting behavior through field metadata, so generic code must not assume that a non-NULL `hfinfo->strings` pointer is a value-string table.

Merged master MR !5424, authored by John Thacker, removes combinations such as `BASE_HEX|BASE_CUSTOM` because the modes are mutually exclusive. Merged master MR !5419, also authored by John Thacker, fixes a crash in custom-column formatting by skipping `hf_try_val_to_str()` / `hf_try_val64_to_str()` when the field display mode is `BASE_CUSTOM`; in that mode the metadata can describe a custom formatter rather than a value-string mapping. Stable backport !5423 carries the crash fix.

**Implementation rule:** choose `BASE_CUSTOM` by itself when presentation is callback-driven. Before interpreting `hfinfo->strings`, branch on the display mode and use only the metadata contract valid for that mode.

**Review rule:** test ordinary value-string fields and custom-format fields separately. A pointer being non-NULL does not establish what kind of object it denotes; the display mode supplies that semantic discriminator.

**Confidence:** Very high. Two merged master fixes authored by John Thacker, with an accepted stable-branch backport.
