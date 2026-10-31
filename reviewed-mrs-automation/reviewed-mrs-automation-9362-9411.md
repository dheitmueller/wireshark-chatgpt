# Wireshark MR review automation: !9411 through !9362

Review date: 2026-10-31. Corpus: `dheitmueller/wireshark-corpus-mrs` at exact commit `ddcaa22b51c68f594e425a23388c3a2086813054`. Notebook starting commit: `4c4d0e9124af423fb6d50a15ad472db829a68e40`. Direction: descending from highest-numbered unreviewed MR available.

**Reviewed: exactly 50 distinct corpus MRs, !9411 to !9362 inclusive, 48 merged and 2 closed without merging (!9371, !9362).** All 50 corpus JSON records, their included discussion notes, and available per-file diffs were examined. Some large/binary diffs are omitted or empty in the corpus (in particular the PFCP implementation in !9368), which is recorded as an evidence limit. No builds, fuzzers, or runtime test executions were performed.

## Exact membership and duplicate-avoidance verification

The notebook `main` tree was inventoried, including its **358 per-run/supplemental files** in `reviewed-mrs-automation/` and root `reviewed-mrs.md`. The adjacent exact ledger `reviewed-mrs-automation-9412-9461.md` ends with !9412; every candidate !9411 through !9362 exists in the corpus. Existing ledger filename ranges account for the higher-numbered frontier, with exceptional membership separately checked for !26566 (`reviewed-mrs-automation-15099-15147-plus-26566.md`) and the !25759-!25827 backfill (the aggregate `reviewed-mrs.md`, supplemental `reviewed-mrs-automation.md`, and `reviewed-mrs-automation-25827-backfill-50.md` collectively explicitly mention every member of that window). Ranges were not treated as proof of reviewed membership for the exceptions. No aggregate or supplemental tracking identifies any of this batch's 50 MR numbers as reviewed. The historic `reviewed-mrs-automation-17571-17620.md` was read and contains **50 unique explicit table entries, !17571-!17620**; it is preserved unchanged.

## Exact reviewed MR list and assessments

| MR | State | Review depth | Evidence and durable assessment |
| --- | --- | --- | --- |
| !9411 | merged | Discussion-focused | BGP-LS IPv6 Neighbor TLV: compare the wire length with the length constant, not the type ID; Jaap Keuter/Alexis La Goutte requested CI before approval. |
| !9410 | merged | Scanned | MSYS2 README now documents the PKGBUILD-based package workflow rather than only local dependency setup. |
| !9409 | merged | Deep | UDS hands low service IDs (<0x10) to an OBD-II-specific registered handler with an explicit opt-out preference, enabling overlapping protocols on one connection. |
| !9408 | merged | Scanned | OBD-II service 0x04 can carry no parameter byte; remove the false mandatory-PID assumption on requests/responses. |
| !9407 | merged | Scanned | Introduces an MSYS2 PKGBUILD with explicit dependency and MinGW variant packaging. |
| !9406 | merged | Scanned | release-3.6 backport of !9404 Opus TOC duration table correction. |
| !9405 | merged | Scanned | release-4.0 backport of !9404 Opus TOC duration table correction. |
| !9404 | merged | Scanned | Opus config 0-11 durations corrected to RFC 6716 values 10/20/40/60 ms instead of 10/20/30/40 ms. |
| !9403 | merged | Scanned | Corrects two MySQL user-facing field/expert diagnostic typos. |
| !9402 | merged | Scanned | PFCP QFI is six bits: use field mask 0x3f, not 0x7f; schema bit widths must match hf masks. |
| !9401 | merged | Scanned | release-4.0 backport of !9379 macOS DMG donation-page packaging. |
| !9400 | merged | Scanned | Falco plugin configuration renamed from SINSP_PLUGINS to FALCO_PLUGINS; build docs and copy target migrate together. |
| !9399 | merged | Deep | Gerald Combs removes the arbitrary SCAP_TIMEOUT shutdown introduced by !9393; filtered/time-out events are normal nonfatal outcomes. |
| !9398 | merged | Deep | Gerald Combs removes guessed Falco plugin discovery because there is no standard plugin directory; build consumer supplies explicit plugin paths. |
| !9397 | merged | Deep | MSYS2 installed vs build-tree runtime path discovery, native path conversion, and system Qt plugin search are distinct from bundled Windows distribution. |
| !9396 | merged | Scanned | release-3.6 backport of !9394 Qt coloring-rule persistence before dialog acceptance. |
| !9395 | merged | Scanned | release-4.0 backport of !9394 Qt coloring-rule persistence before dialog acceptance. |
| !9394 | merged | Deep | John Thacker removes duplicate accepted()->accept wiring; write coloring rules first and call done(Accepted/Rejected) based on result. |
| !9393 | merged | Scanned / superseded | Gerald Combs adds SCAP event result handling; timeout-abort portion was removed in later merged !9399, so do not promote that part. |
| !9392 | merged | Discussion-focused | Trim CPU brand presentation whitespace. Guy Harris explains CPUID brand strings may have leading blanks and Windows Registry may preserve them. |
| !9391 | merged | Scanned | Corrects Diameter 3GPP bit-field display label to match the registered semantic field. |
| !9390 | merged | Scanned | F1AP ASN.1 sources, .cnf/template and generated dissector updated to 3GPP v17.3.0 together. |
| !9389 | merged | Scanned | E1AP ASN.1 definition, .cnf/template, and generated code updated together to v17.3.0. |
| !9388 | merged | Scanned | NRPPa ASN.1 source/config/template and generated dissector updated together to v17.3.0. |
| !9387 | merged | Scanned | XnAP ASN.1 source/config/template and generated dissector updated together to v17.3.0. |
| !9386 | merged | Discussion-focused | Keysight NetFlow enterprise fields updated; Martin Mathieson required consistent whitespace, not mixed tabs/spaces. |
| !9385 | merged | Scanned | ASN.1 extraction tooling recognizes M2AP/M3AP specification identifiers. |
| !9384 | merged | Deep | CIP vendor registry names: Alexis La Goutte accepted canonical ODVA spelling, Roland Knall discussed outdated company names, Gerald Combs affirmed UTF-8 literals. |
| !9383 | merged | Scanned | NGAP 3GPP v17.3.0 ASN.1 source/config/template and generated code updated coherently. |
| !9382 | merged | Scanned | M2AP 3GPP v17.0.1 ASN.1 source/config/template and generated code updated coherently. |
| !9381 | merged | Scanned | X2AP 3GPP v17.3.0 ASN.1 source/config/template and generated code updated coherently. |
| !9380 | merged | Scanned | S1AP 3GPP v17.3.0 ASN.1 source/config/template and generated code updated coherently. |
| !9379 | merged | Scanned | Adds donation page/document/background to macOS DMG; stable backport !9401 corroborates. |
| !9378 | merged | Deep | dftest --types prints filter VM operand/field types with explicit DF_DUMP_SHOW_FTYPE flag, preserving default dump output. |
| !9377 | merged | Deep | Gerald Combs fixes CI pre-commit base to use CI_COMMIT_SHA instead of local HEAD, making job checkout reference explicit. |
| !9376 | merged | Deep | Gerald Combs prefers tvb_memcpy(tvb, dst, off, len) over memcpy(dst, tvb_get_ptr(...), len); updated ASN.1 .cnf and emitted E2AP code plus LoRaWAN. |
| !9375 | merged | Scanned | Narrow LLS table-ID variable to guint8 to match tvb_get_guint8 and avoid MSVC conversion warning. |
| !9374 | merged | Deep | ALP MPEG-TS repacking uses tvb_memcpy and decrements TS-packet count before processing subsequent packets to avoid one extra iteration. |
| !9373 | merged | Deep | Guy Harris replaces cloning/mutating packet_info for tunneled EAP/TLS conversation identity with conversation_set_conv_addr_port_endpoints on the real packet. |
| !9372 | merged | Deep / regression caveat | HTTP/2 calculated flow-control windows; Alexis La Goutte flagged missing no-nghttp2 build and null-flow path; John Thacker reported issue #18815, corrected later by !9534. |
| !9371 | closed | Scanned / down-weighted | Unmerged draft to use NDEBUG uniformly; do not treat as accepted policy. Merged !9365 instead chose WS_DEBUG/WS_ASSERT positive macros. |
| !9370 | merged | Scanned | NAS 5GS dissector specification v17.9.0 update with new protocol fields. |
| !9369 | merged | Deep | MySQL compressed protocol: uncompressed-length zero denotes an uncompressed payload despite compression negotiation; iterates nested PDUs from decompressed tvbuff. |
| !9368 | merged | Deep / partial diff | PFCP enterprise IE refactor: Joakim Karlsson and Anders Broman require retaining external vendor dissector registration; final patch exposes callback handle data and preserves table. Giant PFCP diff omitted in corpus. |
| !9367 | merged | Scanned | Martin Mathieson defines distinct exit codes for command line, interface, file, and display-filter failures, with test expectations updated. |
| !9366 | merged | Scanned | Fix GTPv2 EPS NAS context field mask to 0x03 (two bits). |
| !9365 | merged | Deep | João Valverde uses positive WS_DEBUG and WS_ASSERT flags in CMake/source; docs explicitly warn that assertions must have no side effects. |
| !9364 | merged | Deep | FIPS-required libgcrypt rejects RIPEMD160; after Guy Harris/Gerald Combs discussion capinfos drops the algorithm and updates CLI/manual, not policy bypass. |
| !9363 | merged | Scanned | PFCP identical grouped-IE decoders consolidated to one generic implementation, removing repetitive wrappers. |
| !9362 | closed | Discussion-focused / down-weighted | HTTP/3 static-table QPACK HEADERS proposal closed; Ryan Doyle and Alexis La Goutte directed work to already existing !9330; not accepted implementation evidence. |

## Weighting and follow-up

Merged master implementations and maintainer-review requirements carry the most weight; stable cherry-picks corroborate them. Accepted changes later corrected are *not* presumed good precedents: !9393's timeout policy was removed in !9399, while !9372's HTTP/2 context bug was identified by John Thacker and corrected by later !9534. Closed !9371 and !9362 are not accepted architecture; the latter was superseded by !9330. Strongest direct review evidence: Guy Harris (!9373, !9364, !9392), Gerald Combs (!9376, !9364, !9384), John Thacker (!9372, !9394), Anders Broman / Joakim Karlsson (!9368), Alexis La Goutte (!9372, !9384), and Martin Mathieson (!9386).

Next expected eligible MR after rebuilding the reviewed set from the then-current repository: **!9361**, provided the corpus snapshot still contains it and no new higher-numbered unreviewed records arrive. Keep the recurring automation running.
