# Automated Wireshark MR review: !24959–!25008

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: reviewed all available tracking in `reviewed-mrs-automation/` and `reviewed-mrs.md`, built the already-reviewed MR set, and selected the 50 highest-numbered corpus MRs not present in that set. The previously reviewed historical batch !17571–!17620 remains preserved and counted as reviewed.

Reviewed exactly 50 MRs in descending order:

!25008, !25007, !25006, !25005, !25004, !25003, !25002, !25001, !25000, !24999,
!24998, !24997, !24996, !24995, !24994, !24993, !24992, !24991, !24990, !24989,
!24988, !24987, !24986, !24985, !24984, !24983, !24982, !24981, !24980, !24979,
!24978, !24977, !24976, !24975, !24974, !24973, !24972, !24971, !24970, !24969,
!24968, !24967, !24966, !24965, !24964, !24963, !24962, !24961, !24960, !24959.

## Review notes

- !25008 — merged DMX output-label cleanup; no durable convention.
- !25007 — merged bulk Doxygen header cleanup; no new convention.
- !25006 — merged John Thacker TCP/UDP/DCCP Follow work; reinforces layer-aware stream identity under tunneling rather than reconstructing identity from final `packet_info` endpoint state.
- !25005 — merged John Thacker sanitizer build correction, approved by Gerald Combs; promoted the rule that hardening such as `_FORTIFY_SOURCE` must not mask ASan/TSan diagnostics.
- !25004 — merged Qt Find in Packet feature; feature-specific implementation, no new durable rule promoted.
- !25003 — merged release backport for persistent macOS ccache storage.
- !25002 — merged release backport for persistent macOS ccache storage.
- !25001 — merged release preparation; no reusable engineering convention.
- !25000 — merged release preparation; no reusable engineering convention.
- !24999 — merged DECT NR endpoint-mux dissector-table support; useful extensibility example, no distinct new rule needed.
- !24998 — merged master macOS ccache storage change; CI cache persistence example.
- !24997 — merged release build update; no new convention.
- !24996 — merged release build update; no new convention.
- !24995 — merged public-header test correction; reinforces testing headers in the consumer/build context required by their contract rather than assuming every public header is standalone under one generic harness.
- !24994 — merged capture warning cleanup; file-local state made static/conditional as appropriate.
- !24993 — merged Kafka compact-string overflow backport; reinforces bounded varints and checked length arithmetic.
- !24992 — merged Kafka compact-string overflow backport; same accepted arithmetic-safety pattern.
- !24991 — merged John Thacker fuzzing behavior change; promoted the rule that fuzz-only non-progress/resource guards should fail strongly enough for the fuzz harness to classify and retain the input.
- !24990 — merged BACapp backport; validates combinations of individually optional fields with a protocol-level at-least-one requirement.
- !24989 — merged BACapp backport; same parser-validation lesson; John Thacker also notes BACnet encoding differs enough from ASN.1 that generic ASN.1 tooling is not automatically appropriate.
- !24988 — merged John Thacker Kafka master fix; strong reinforcement for avoiding signed-overflow UB, bounding compact varints, and causing tvbuff bounds checks before unsafe C arithmetic.
- !24987 — merged source-checker integration; reinforces use of project checker entry points.
- !24986 — merged Kafka arithmetic-safety backport.
- !24985 — merged Kafka arithmetic-safety backport.
- !24984 — merged John Thacker BACapp master fix; strong parser-validation example for optional-field combinations and expert information on invalid combinations.
- !24983 — merged John Thacker Asciidoctor CI correction; promoted the rule that the diagnostic severity capable of failing a job must remain visible in the job log.
- !24982 — merged Guy Harris declaration cleanup, reviewed/merged by John Thacker and approved by Gerald Combs; reinforces authoritative headers for externally visible declarations.
- !24981 — merged `gboolean` modernization after maintainer review; promoted the rule that dependency-defined scalar types must be preserved at external API boundaries even when internal code uses C99 `bool`.
- !24980 — merged Gerald Combs weekly-update tooling change; network-dependent recurring generators should have bounded retry behavior before automation depends on them.
- !24979 — merged release preparation; no new convention.
- !24978 — merged release preparation; no new convention.
- !24977 — merged Guy Harris plugin warning enforcement, reviewed/merged by John Thacker; strong support for centralized plugin export declarations and applying declaration warnings to plugin code too.
- !24976 — merged Doxygen header conversion; no new durable rule.
- !24975 — merged JSON array key-lookup support; parser-specific implementation, no new convention promoted.
- !24974 — closed, not merged; broad fixed-endian signed-getter replacement. Guy Harris's later performance/ABI discussion is useful design evidence but deliberately down-weighted because this bulk change was not accepted.
- !24973 — merged nl80211 generator source-URL release backport.
- !24972 — merged nl80211 generator source-URL release backport.
- !24971 — merged SAP HDB negative-length backport; reinforces parser progress: packet-controlled signed lengths must not move offsets backward.
- !24970 — merged SAP HDB negative-length backport; same parser-progress rule.
- !24969 — closed, not merged; broad fixed-endian unsigned-getter replacement. Reviewer preference is informative but this bulk proposal is deliberately down-weighted as non-accepted architecture.
- !24968 — merged John Thacker allocator-lifetime fix; column strings must live in `pinfo->pool` rather than a packet scope that can expire too early under some allocators.
- !24967 — merged spelling cleanup; review caught a potentially misleading apparent preference change, but final change was non-semantic.
- !24966 — merged NVMe encoding correction; local review prefers the fixed-endian getter when endianness is statically known, but no blanket rule promoted because the related bulk conversions were later closed.
- !24965 — merged AMP encoding correction; same fixed-endian getter preference, treated as local accepted guidance rather than a universal convention.
- !24964 — merged SGP.22 generated ASN.1 update; no distinct new rule.
- !24963 — merged PFCP cleanup; Jaap Keuter's review reinforces defining related constants/sentinels near the concepts they constrain so future additions do not miss them.
- !24962 — merged Guy Harris missing-declaration/plugin-export work, reviewed/merged by John Thacker; strong support for centralizing the plugin export contract in a shared header that also validates required build mode.
- !24961 — merged Qt status-bar styling fix; no new durable convention.
- !24960 — merged Clang Analyzer dead-store cleanup; no new durable convention.
- !24959 — merged release backport replacing the tool-reserved `RCLONE_CONFIG` variable with project-owned `WS_RCLONE_REMOTE_OPTS`; reinforces avoiding semantic collisions with dependency-reserved environment-variable names.

## Notebook changes promoted from this batch

- `ci-tooling-conventions.md`: sanitizer builds must not enable hardening that masks sanitizer findings (!25005).
- `ci-tooling-conventions.md`: fuzz-only guardrails should fail in a form the fuzz harness can classify (!24991).
- `ci-tooling-conventions.md`: do not suppress the diagnostic severity that makes a CI tool fail (!24983).
- `build-conventions.md`: preserve dependency-defined scalar types at API call boundaries (!24981).

Merged work was weighted more heavily than closed/superseded proposals. In particular, !24974 and !24969 were retained as lower-weight design/review evidence rather than treated as accepted conventions.