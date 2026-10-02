# Review synthesis — !2011–!2060

This batch reinforces several high-confidence Wireshark conventions.

## Numeric field presentation

The strongest evidence is !2045, where Guy Harris explicitly distinguishes identifiers such as addresses and keys from quantities such as sizes, counts, lengths, and sequence numbers. Quantities should be displayed decimal-first; if hexadecimal is useful too, use `BASE_DEC_HEX`, not hex-first. Guy then applied that policy systematically in merged !2057 and !2058 across InfiniBand, iSCSI, NVMe, and SCSI.

!2055 adds a separate field-modeling boundary: an ordinary unmasked `FT_BOOLEAN` has no numeric base, and the display argument is only reused as a containing-field width for the masked bitfield case. !2046 contains Anders Broman's recommendation to use `proto_tree_add_item_ret_uint()` when the parser also needs a displayed integer value rather than fetching it separately.

## Wiretap block abstraction

Guy Harris-authored !2033, !2036, and !2038 form a coherent design sequence. Wiretap block IDs are internal semantic identities, not pcapng block numbers; multiple on-disk block types may map to one internal abstraction. Names should describe the semantic object, opaque blocks should expose their type through a proper accessor, and a generic fixed-size "custom block" slot registry is weaker than explicit semantic block classes.

## Diagnostics and platform errors

Guy Harris-authored !2013–!2021 and !2028/!2029 establish a robust diagnostic policy. User-facing capture errors should name the human-facing interface, include useful native/runtime capture-library context, and direct bug reports to the component actually in use. Platform messages may be localized, so detect a stable machine pattern (for example a fixed prefix plus native numeric error code) instead of exact English prose.

## Expensive optional initialization

Gerald Combs' merged !2035 demonstrates demand-driven extcap initialization with measurements, not intuition. Unconditional extcap preference registration caused thousands of extra process creations and much slower Windows test runs. TShark now decides from actual command-line needs whether extcap registration is necessary, while preference handling deliberately tolerates an extcap module that was intentionally not fully registered. This is a strong precedent for keeping expensive optional process-backed subsystems lazy.

## UAT lifecycle and validation

Merged !2054 validates malformed ESP keys at the UAT boundary and returns precise error text rather than allowing bad configuration into runtime state. Merged !2053 marks tables that drive dynamic fields with `UAT_AFFECTS_FIELDS` in addition to `UAT_AFFECTS_DISSECTION`. Treat configuration validation, dissection invalidation, and field-registry invalidation as distinct responsibilities.

## Submission dependency discipline

The !2049/!2050 discussion is useful direct Anders Broman workflow evidence: historically, one MR represented one coherent commit, with MRs independent or the dependent MR held until its prerequisite merged. Stacking overlapping changes caused !2050 to be accidentally squashed into !2049, obscuring commit intent. Closed !2041 independently reinforces the topic-branch requirement through Guy Harris, Alexis La Goutte, and Pascal Quantin: do not submit from a protected fork `master` when that prevents the maintainer-edit/rebase workflow.

## Additional qualified evidence

Merged !2040 replaces an ad-hoc `pinfo->private_table` entry with protocol-scoped packet proto-data, a good ownership model for per-packet protocol state. !2044 confirms that a child dissector must interpret offsets in the coordinate system of the TVB it actually receives. !2022 keeps optional-crypto-disabled builds valid by moving declarations inside the feature guards that use them.

Closed !2026 remains useful only as lower-weight negative evidence: a fixed-size C destination must reject `strlen(src) >= sizeof(dst)` so a terminating NUL fits, and Peter Wu requested agreement on API direction before implementation continued. Closed !2012 is similarly lower-weight but useful build-system guidance: Peter Wu preferred repairing the mismatched pkg-config environment and minimizing divergence from upstream CMake Find modules rather than layering a repository workaround over a broken local tool configuration.

No SMPTE ST 291/VANC packet type was encountered.
