# Wireshark MR corpus review: !25309–!25358

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to oldest. This file records exactly the 50 previously unreviewed merge requests examined in this run. The existing ledgers, including the historical !17571–!17620 batch, were consulted before selection.

| MR | Status | Notes |
|---|---|---|
| !25358 | Scanned | release-4.6 backport of the PIDL FT_STRING type-detection fix from !25343. Merged; no additional lesson beyond the master change. |
| !25357 | Scanned | release-4.6 backport of !25352's DCERPC `FINALLY` cleanup. Merged; corroborates exception-safe cleanup without new review discussion. |
| !25356 | Deep | NETLOGON switches decompression to the child-TVBuff ownership API so the decompressed TVB follows the parent TVB lifetime. Merged John Thacker change; reinforces the ownership rule already represented by !25363 and related lifetime guidance. |
| !25355 | Deep | Proto APIs for boolean/float/double/integer/EUI64 fields use unsigned lengths because negative lengths were already invalid and exception-producing. Merged; strongly corroborates the notebook's semantic-domain/API-contract guidance. |
| !25354 | Scanned | O-RAN FH CUS regression fixes around compression-header handling and hidden-filter extents. Merged; protocol-specific corrective work, no distinct durable review rule extracted. |
| !25353 | Deep | Replaces `-1` with zero for a generated/zero-byte FT_BOOLEAN tree item because negative length is invalid for that typed API. Authored by John Thacker and merged by Guy Harris; corroborates semantic length-domain guidance. |
| !25352 | Deep | DCERPC cleanup is moved into a `FINALLY` block so exceptional exits cannot leak temporary state. Merged master change; reinforces exception-safe cleanup/lifetime ownership. |
| !25351 | Deep | HTTP request tracking moves to a file-scope `wmem_list`, aligning container lifetime with the conversation data it owns. Merged; corroborates the existing rule that persistent containers and their members need compatible lifetimes. |
| !25350 | Deep | TCPCL message identity includes the packet data-source index, avoiding collisions when one frame has multiple derived data sources/messages. Merged; strong corroboration of the existing complete-identity-tuple rule for state/reassembly keys. |
| !25349 | Scanned | Documents the software-update/Appcast URL format. Merged documentation-only change; no reusable engineering convention. |
| !25348 | Scanned | stable backport of !25344's child-TVBuff decompression lifetime fix. Merged; no independent lesson. |
| !25347 | Scanned | stable backport of !25344's child-TVBuff decompression lifetime fix. Merged; no independent lesson. |
| !25346 | Deep | DCERPC avoids undefined signed left-shift behavior exposed by UBSan by ensuring arithmetic occurs in the intended unsigned domain. Merged; reinforces sanitizer-driven integer-domain hardening. |
| !25345 | Scanned | Arch Linux CI job repair. Merged build-infrastructure maintenance; no broader convention extracted. |
| !25344 | Deep | John Thacker documents that `tvb_child_uncompress*` is normally preferred because decompressed TVBs should join the compressed TVB's ownership chain; standalone decompression is for deliberate longer-lived ownership. Merged; Guy Harris also prompted auditing another similar dissector. Strong corroboration of existing TVB lifetime guidance. |
| !25343 | Deep | PIDL generation recognizes additional nested string-containing NDR types as FT_STRING so generated field types match the string dissector APIs that consume them. Merged; useful generator/API consistency exemplar but no new notebook rule beyond contract consistency. |
| !25342 | Scanned | Wireshark/Stratoshark post-release version bump. Merged release plumbing; no reusable engineering lesson. |
| !25341 | Scanned | Removes an increment identified as a dead store by Clang Analyzer. Merged; static-analysis cleanup corroborates existing warning-analysis workflow. |
| !25340 | Scanned | Wireshark 4.7.1 / Stratoshark 0.10.1 release build preparation. Merged release plumbing; no reusable lesson. |
| !25339 | Deep | RRC and UMTS RLC leak fixes align allocation/free behavior with actual ownership. Merged; reinforces existing memory-lifetime guidance. |
| !25338 | Deep | Proto APIs for time/address/OID-like field types use unsigned lengths because negative values were already unsupported; introduces an unsigned internal helper rather than preserving a misleading signed contract. Merged; reinforces semantic API-domain guidance. |
| !25337 | Deep | E2AP resets the entire gNB table between capture files rather than leaving stale entries/fields from the previous capture. Merged Martin Mathieson change; corroborates existing capture/session state-reset ownership guidance. |
| !25336 | Scanned | SGP.32 tag-list expansion. Merged protocol-data update; no broader review convention. |
| !25335 | Scanned | SGP.22 tag-list expansion. Merged protocol-data update; no broader review convention. |
| !25334 | Scanned | Adds CBOR diagnostic-notation display options for human/tool interoperability. Merged feature; no distinct durable reviewer correction identified. |
| !25333 | Deep | Fixes `proto_tree_add_uint_format` callers that supplied length `-1`, which is not valid for integer fields. Merged; corroborates semantic length-domain guidance. |
| !25332 | Scanned | Suppresses repeated Qt warnings after dumpcap has already reported that no interfaces expose statistics. Merged; UI/logging behavior improvement, no new general rule extracted. |
| !25331 | Scanned | Corrects dumpcap error wording so a missing statistics list is not mislabeled as proof that no capture interfaces exist. Merged; reinforces precise diagnostic semantics. |
| !25330 | Scanned | Packages `libunwind.dll` with Stratoshark after the Windows Falco-plugin implementation changed runtime dependencies. Merged packaging fix; no broader convention beyond dependency closure. |
| !25329 | Scanned | Registers CESoPSN with the CESoETH ECID dissector table so it can be selected for dissection. Merged; standard table-based dispatch, already covered by extensibility guidance. |
| !25328 | Scanned | Deduplicates repeated `value_string` tables across dissectors. Merged maintainability cleanup; no additional rule. |
| !25327 | Scanned | Temporarily disables nonfunctional Coverity Scan CI jobs. Merged CI maintenance; no durable coding/review rule. |
| !25326 | Deep | John Thacker changes proto API offsets to unsigned because modern tvbuff APIs no longer accept negative offsets and `field_info` already stores them unsigned. Merged; strong corroboration of semantic integer domains and making API signatures express real contracts. |
| !25325 | Scanned | Restores CI-generated Wireshark/Stratoshark version variables after an earlier pipeline change. Merged; CI compatibility maintenance. |
| !25324 | Discussion-focused | Adds editcap SCTP DATA-chunk splitting and provenance comments. Merged, but John Thacker later identified incorrect handling of truncated packets; author pointed to follow-up !25600. Important negative evidence that packet-transform code must distinguish captured bytes from advertised/original lengths, a rule already promoted from !25600. |
| !25323 | Scanned | Implements previously stubbed NAS-5GS information elements from TS 24.501. Merged protocol coverage; no distinct reusable review lesson extracted. |
| !25322 | Scanned | RSVP cleanup driven by project check-script warnings. Merged; corroborates existing pre-submit checker guidance. |
| !25321 | Discussion-focused (closed/superseded) | Early NVMe-MI split-by-type + Control Primitive submission. Closed unmerged; the later merged !25763 is the stronger architectural evidence and already records the accepted split rationale. Do not treat this version as the implementation exemplar. |
| !25320 | Scanned | Defines the correct log domain for direct wslog calls in the Qt interface-list manager. Merged; localized logging integration fix. |
| !25319 | Scanned | Adds BSSMAP-LE Connectionless Information message dissection. Merged protocol feature; no durable review correction extracted. |
| !25318 | Deep | dumpcap returns the dedicated `WS_EXIT_NO_INTERFACES` status consistently when interface/statistics enumeration succeeds but produces no interfaces, instead of returning a generic numeric/invalid-interface code. Merged John Thacker change; reinforces explicit semantic status contracts. |
| !25317 | Scanned | CESoETH gains per-ECID Decode As selection for alternate pseudowire encapsulations. Merged; standard dissector-table/Decode-As extensibility pattern. |
| !25316 | Scanned | Adds minimum supported macOS version to generated release-info metadata. Merged packaging/update metadata work; no broader convention. |
| !25315 | Scanned | Builds/installs translations explicitly for MinGW where windeployqt does not perform the native-Windows behavior. Merged platform packaging work; reinforces capability/behavior-based platform handling already covered elsewhere. |
| !25314 | Scanned (closed draft) | Experimental GitLab CI rclone PAT draft. Closed unmerged; no accepted implementation or durable review evidence. |
| !25313 | Scanned | MinGW packaging tolerates a new benign windeployqt output line. Merged backport/packaging compatibility work; no broader lesson. |
| !25312 | Scanned | MinGW packaging tolerates a new benign windeployqt output line. Merged backport/packaging compatibility work; no broader lesson. |
| !25311 | Scanned | MinGW packaging tolerates a new benign windeployqt output line. Merged master/stable packaging compatibility work; no broader lesson. |
| !25310 | Scanned | Redesigns Qt Font & Colors theme preview around theme tokens rather than independent light/dark detection. Merged GUI feature; consistent with existing single-source-of-truth UI-state guidance. |
| !25309 | Scanned | Fixes mismatched braces in the Code Lines and Data CI job. Merged fixup; no reusable engineering lesson. |

## Durable conclusions

No new core convention was promoted in this run because the strongest evidence independently corroborates rules already present in the notebook:

- API integer types should model the real semantic domain rather than preserve invalid sentinel possibilities (!25326, !25333, !25338, !25353, !25355).
- Stateful/reassembly identity must include every discriminator needed to distinguish logically separate messages, including derived data-source identity when relevant (!25350).
- Capture/session-scoped state must be completely reset at its lifecycle boundary (!25337).
- Derived TVBs should normally participate in the parent's ownership chain unless intentionally given a longer lifetime (!25344, !25347, !25348, !25356).
- Packet transformation must distinguish captured bytes from advertised/original length; !25324's later truncation issue is directly covered by the rule already extracted from !25600.
- Closed/superseded !25321 is retained as lower-weight historical evidence; merged !25763 remains the accepted NVMe-MI architecture exemplar.
