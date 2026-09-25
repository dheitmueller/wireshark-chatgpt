# Automated MR review ledger: !9562–!9612 (mr_9604 absent)

Reviewed 2026-09-25 using GPT-5.6 Sol.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: newest available previously unreviewed MRs toward older MRs.

Before selection, the available notebook review tracking was consulted on the latest accumulated review branch, including `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, the preceding exact per-run ledgers, and the historical `reviewed-mrs-automation-17571-17620.md` ledger. The historical !17571–!17620 file still contains exactly 50 reviewed MRs. Candidate selection was by explicit MR-number membership, not inferred range coverage.

The corpus has no `mr_9604.json` at the reviewed commit. The fifty highest-numbered available corpus MRs not already represented in tracking are therefore the exact set below. Status mix: **48 merged, 2 closed/unmerged (!9603 and !9600)**. Merged !9610 was subsequently reverted after macOS breakage and is down-weighted as an implementation exemplar.

## Exact reviewed MR set

!9612, !9611, !9610, !9609, !9608, !9607, !9606, !9605, !9603, !9602,
!9601, !9600, !9599, !9598, !9597, !9596, !9595, !9594, !9593, !9592,
!9591, !9590, !9589, !9588, !9587, !9586, !9585, !9584, !9583, !9582,
!9581, !9580, !9579, !9578, !9577, !9576, !9575, !9574, !9573, !9572,
!9571, !9570, !9569, !9568, !9567, !9566, !9565, !9564, !9563, !9562

Count: **50 unique MRs**. Maximum: **!9612**. Minimum: **!9562**. Missing corpus entry inside the span: **!9604**.

## Per-MR review notes

| MR | Outcome | Depth | Notes |
|---|---|---|---|
| !9612 | merged | Scanned | Guy Harris release-4.0 TWAMP backport correcting MBZ field widths to FT_UINT16. |
| !9611 | merged | Scanned | Guy Harris release-3.6 TWAMP backport of the same field-width correction. |
| !9610 | merged, later reverted | Deep / down-weighted | Linux relocatable-RPATH change; Guy Harris discusses loader/$ORIGIN and privileged-program constraints. macOS CI exposed duplicate static/dynamic wsutil linkage and Gerald Combs initiated !9647 revert. |
| !9609 | merged | Scanned | BGP EVPN Add-Path support; protocol-specific. |
| !9608 | merged | Deep | John Thacker makes pcapng Name Resolution Blocks grow with reading and preserves them through editcap/mergecap with seen cursors. |
| !9607 | merged | Discussion-focused | Unix configuration-path relocation; Guy Harris and Gerald Combs discuss bundle/install layouts and AppImage; macOS tests exposed path-layout assumptions later refined. |
| !9606 | merged | Scanned | HTTP/2 stores request/response frame numbers and exposes generated cross-reference fields. |
| !9605 | merged | Scanned | SAP expert-info abbreviations renamed to avoid incompatible duplicate hf/expert filter identifiers. |
| !9603 | closed/unmerged | Scanned / down-weighted | Delivery Optimization dissector proposal; review asked that referenced documentation be available before merge. |
| !9602 | merged | Scanned | Spelling/filter-abbreviation cleanup. |
| !9601 | merged | Discussion-focused | RTPS pre-shared-secret protection; review fixed ordering/checksum, dead-store and naming issues. |
| !9600 | closed/unmerged | Discussion-focused / down-weighted | Direct German translation edit redirected by Alexis La Goutte to Wireshark's Transifex workflow. |
| !9599 | merged | Scanned | Guy Harris stable backport making file-extension matching case-insensitive. |
| !9598 | merged | Scanned | Guy Harris stable backport of case-insensitive file-extension matching. |
| !9597 | merged | Deep | Pascal Quantin drives Exported-PDU TCP metadata toward FT_BOOLEAN/ret_boolean for zero-vs-nonzero semantics and correct ret_uint output typing. |
| !9596 | merged | Deep | Wiretap Exported-PDU construction helpers clarify buffer append semantics: reserve capacity, write at logical end, advance length only after successful production. |
| !9595 | merged | Discussion-focused | GREBonding extension; review requests filterable fields, lower-case abbreviations and whitespace cleanup. |
| !9594 | merged | Discussion-focused | File-extension matching; Pascal Quantin questions repeated comparison cost and contributor uses existing ascii_strdown_inplace helper. |
| !9593 | merged | Deep | Martin Mathieson fixes check_typed_item_calls.py regexes so legal whitespace before '(' does not bypass call recognition. |
| !9592 | merged | Deep | TWAMP width fix; Martin Mathieson explicitly says checker missed it solely because of whitespace between function name and '('. |
| !9591 | merged | Scanned | UDS error service table extended with OBD services. |
| !9590 | merged | Scanned | UDS NRC/SID label typo correction. |
| !9589 | merged | Scanned | O-RAN value/table and beamforming subtree updates. |
| !9588 | merged | Scanned | SMB2 Lock Sequence field label correction. |
| !9587 | merged | Scanned | BTLE value_string copy/paste correction. |
| !9586 | merged | Discussion-focused | MySQL mid-conversation capability inference; Alexis suggests preference-based fallback rather than an implicit guess. |
| !9585 | merged | Scanned | MySQL length-encoded integer display/noise cleanup. |
| !9584 | merged | Scanned | Automatic generated-data/translation update; low semantic weight. |
| !9583 | merged | Deep | SAPRFC dissector review catches duplicate abbreviations, dead stores, wrong table/port semantics and bad val_to_str fallbacks; merged after extended cleanup. |
| !9582 | merged | Scanned | SAPRouter credentials tap; contributor supplies sample capture. |
| !9581 | merged | Deep | New SAPDiag dissector review catches typed-item length mismatches, dead stores, duplicate incompatible fields, spelling and static-scope issues. |
| !9580 | merged | Scanned | Release-note path correction. |
| !9579 | merged | Scanned | MySQL incomplete-dissector warning fix. |
| !9578 | merged | Scanned | Automatic release/generated-data update; low semantic weight. |
| !9577 | merged | Scanned | Automatic release/generated-data update; low semantic weight. |
| !9576 | merged | Scanned | IEEE1905 capability update with ordinary formatting/commit-message cleanup. |
| !9575 | merged | Scanned | John Thacker updates openSUSE CI container from 15.3 to 15.4. |
| !9574 | merged | Discussion-focused | John Thacker modernizes RPM dependency/build conditionals and removes obsolete distribution cruft. |
| !9573 | merged | Deep | John Thacker makes pcapng_open consume all initial built-in internal blocks while conservatively stopping on plugin/custom block types whose ownership cannot be known. |
| !9572 | merged | Scanned | John Thacker adapts tests to Python 3.11 unittest outcome internals. |
| !9571 | merged | Scanned | Release-note entry for Development install component. |
| !9570 | merged | Scanned | WPS Multi-AP flag additions; review catches numeric style, whitespace and commit-message length. |
| !9569 | merged | Discussion-focused | openSUSE RPM build fix; John Thacker advocates testing with distro mock builds rather than blind packaging iterations and follows with !9574. |
| !9568 | merged | Scanned | Debian install-headers handling for Ninja. |
| !9567 | merged | Scanned | Qt pre-5.14 compatibility fix. |
| !9566 | merged | Scanned | Loads user extcaps from newly defined personal extcap directory. |
| !9565 | merged | Scanned | RPM docdir packaging correction. |
| !9564 | merged | Discussion-focused | MySQL caching_sha2_password decoding; review checks unused state, initialization and enum coverage. |
| !9563 | merged | Discussion-focused | Logray Follow Stream removal; John Thacker catches unrelated accidental commented-out behavior before merge. |
| !9562 | merged | Deep | Martin Mathieson extends typed-item checking to validate proto_tree_add_item encoding arguments and fixes many real value-vs-encoding call mistakes. |
