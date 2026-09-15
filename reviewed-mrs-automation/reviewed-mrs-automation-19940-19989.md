# Automated MR review: !19940–!19989

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection method: rebuilt the reviewed set from the existing per-run ledgers and persistent tracking, preserving the historical !17571–!17620 batch and the previously backfilled !22966. The corpus revision is unchanged from the preceding run, so no new higher-numbered corpus holes were introduced. The next fifty highest-numbered unreviewed corpus objects are the contiguous set !19989 through !19940.

Reviewed exactly 50 MRs:

`19989 19988 19987 19986 19985 19984 19983 19982 19981 19980 19979 19978 19977 19976 19975 19974 19973 19972 19971 19970 19969 19968 19967 19966 19965 19964 19963 19962 19961 19960 19959 19958 19957 19956 19955 19954 19953 19952 19951 19950 19949 19948 19947 19946 19945 19944 19943 19942 19941 19940`

## Durable findings

- **!19957, !19979, !19980 — generic extension dispatch (very high confidence).** !19979 reshaped custom-binary-option handling to the normal dissector callback signature; !19980 moved Netflix BBLog semantics out of common `packet-frame.c` into `packet-bblog.c` and dispatched custom binary options through a dissector table. In !19957 review, Guy Harris explicitly raised module registration and lookup tables as the direction for non-core record types instead of growing core switch statements. Added to `pcapng-extension-architecture-conventions.md`.
- **!19976 — keep semantic diagnostics outside optional presentation work (very high confidence).** Guy Harris consolidated frame dissection around one code path, cached whether frame fields were referenced, and deliberately kept expert-info generation outside that optimization gate. This reinforces the existing principle that performance shortcuts for unreferenced presentation fields must not suppress diagnostics or semantic side effects.
- **!19950 — source representation must remain visible to project tooling (high confidence).** MQ value-string macros were undone because Wireshark's validation tooling could not inspect value strings hidden behind macros; a pseudo-value-string relying on pointer tricks was replaced by a proper hash table. This reinforces existing static-analysis/tooling conventions: abstractions should not obscure declarative structures from repository validation tools, and data structures should match their semantic contract rather than exploit representation accidents.
- **!19989 — protocol-field abbreviations must not collide across incompatible field types (high confidence).** John Thacker removed an FT_NONE field that duplicated the protocol abbreviation and triggered `ENABLE_CHECKHF_CONFLICT`. This is strong corroboration for registration-time field validation and keeping abbreviations unique/semantically consistent.
- **!19982 — CI hooks must preserve the execution semantics of the tool they emulate (high confidence).** The Commit Check adjustment recognizes that pre-commit normally runs before a commit and therefore needs special handling for the first commit. This reinforces testing tools in the same state/phase assumptions they have in their native invocation.
- **!19965 — error paths need the same resource ownership discipline as success paths (high confidence).** The WSLua gcrypt change fixes leaks specifically on tested error paths, reinforcing existing lifecycle/partial-failure cleanup guidance.
- **!19962 — protocol state machines must represent legal repeated transitions (high confidence).** QUIC permits multiple Retry packets under defined constraints; state must not encode an overly strict one-time transition when the protocol allows repetition.
- **!19969 — diagnostics should not depend on optional tree construction (high confidence).** MQ cleanup ensured expert information is created even when the protocol tree is absent, independently corroborating the !19976 lesson.
- **!19941 / !19951 — encoding flags must match field type and API semantics (high confidence).** These merged fixes continue the encoding-value cleanup series already captured by the notebook; no duplicate convention was added.
- **!19970, !19981, !19988 — SSH extension support and parser factoring (high confidence).** These merged John Thacker changes broaden public-key/host-bound authentication handling and strict-KEX state behavior. They corroborate existing SSH/state and reusable parser guidance rather than introducing a new general convention.

## Lower-yield / protocol-specific items

The remainder of the batch consists primarily of protocol-specific corrections and updates (TLS/ML-KEM draft churn, Zigbee Touchlink, Profinet, Diameter, NGAP, TTL, LLDP, certificate-fragment presentation), Qt/theme behavior, documentation, automated data updates, build/packaging work, and mechanical cleanup. These were reviewed for reusable lessons but did not justify additional notebook rules beyond the corroborations above.

Merged results were weighted above abandoned/superseded work; direct Guy Harris and John Thacker architecture/review evidence received the highest weight.