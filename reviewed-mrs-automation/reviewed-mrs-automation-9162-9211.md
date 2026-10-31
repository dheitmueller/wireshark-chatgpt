# Wireshark MR automation review ledger — !9211 through !9162

Reviewed **2026-10-31** against corpus `dheitmueller/wireshark-corpus-mrs` **commit `ddcaa22b51c68f594e425a23388c3a2086813054`**. Notebook starting head: `ee1fc3fd4b6fdc531114e10c1c1e92973b01e4a7`. All outcomes below are **historical snapshot outcomes**, not new live GitLab status checks.

**Exact reviewed membership (descending, 50 distinct):** !9211, !9210, !9209, !9208, !9207, !9206, !9205, !9204, !9203, !9202, !9201, !9200, !9199, !9198, !9197, !9196, !9195, !9194, !9193, !9192, !9191, !9190, !9189, !9188, !9187, !9186, !9185, !9184, !9183, !9182, !9181, !9180, !9179, !9178, !9177, !9176, !9175, !9174, !9173, !9172, !9171, !9170, !9169, !9168, !9167, !9166, !9165, !9164, !9163, !9162.

**Outcomes:** 45 merged, 4 closed without merge, 1 open. Merged implementations were weighted more heavily than closed or unmerged proposals; direct maintainer feedback (especially Guy Harris, Martin Mathieson, John Thacker, João Valverde, Alexis La Goutte) was weighted according to its substance. An accepted MR with a post-merge correction was not assumed to represent a flawless reference.

## Selection and tracking verification

Before selecting, inventoried and **read all 362 files** in `reviewed-mrs-automation/`, including exceptional/skip/noncontiguous/backfill and exact-list files, plus root `reviewed-mrs.md` and the supplemental `reviewed-mrs-automation.md`. Checked the explicit MR-number contents against each candidate, rather than assuming filename ranges represented fully reviewed sets. No candidate !9211–!9162 was marked reviewed; the only prior occurrence of !9211 was the preceding ledger's explicit **unreviewed next-frontier** note. Independently confirmed the preserved historical ledger `reviewed-mrs-automation/reviewed-mrs-automation-17571-17620.md` has exactly 50 distinct table entries, all !17571 through !17620. No previous ledger was modified. All 50 candidate corpus JSON records exist at the pinned commit and their discussions, available diffs, descriptions and outcomes were examined; some large or routine changes were scanned rather than deeply re-reviewed.

## Exact individual assessments

| MR | Corpus outcome | Depth | Diff/discussion assessment |
|---|---|---|---|
| !9211 | merged | Scanned | Gerald fixes IAX2 analysis constructor leak by freeing compiled display filter on no-current-frame path. |
| !9210 | merged | Deep | João enables literal-left comparisons with a bounded type-inference retry; positive and negative syntax tests. |
| !9209 | merged | Scanned | BGP PMSI tunnel attributes distinguish IPv4/IPv6 ingress replication addresses and correct tunnel ID formatting. |
| !9208 | merged | Scanned | Follow-up to Guy's !9194 review: consistent Flex IPv4Address/IPv6Address pattern names. |
| !9207 | merged | Deep | DFVM comparison must accept constant/fvalue as first argument, not assume a register. |
| !9206 | merged | Scanned | NFSv4 directory-entry UTF-8 decoding uses tvb_get_string_enc; stable-branch backport. |
| !9205 | merged | Scanned | Parallel stable-branch NFSv4 UTF-8 string-decoding backport. |
| !9204 | merged | Scanned | Display-filter semantic failure path emits noisy diagnostic and consolidates internal header. |
| !9203 | merged | Deep | Preference syntax errors use GUI-visible report_warning rather than console-only ws_warning. |
| !9202 | merged | Deep | Display-filter grammar preserves parenthesis/braces source spans when reusing AST nodes. |
| !9201 | merged | Deep | John harmonizes IPv4/IPv6/FCWWN resolved custom-column labels and resolution UI controls. |
| !9200 | merged | Scanned | Generated TLS CT list update ignores volatile Last-Modified metadata for meaningful no-op comparisons; backport. |
| !9199 | merged | Scanned | Paired TLS CT generator update with same content-versus-timestamp distinction. |
| !9198 | merged | Scanned | Scheduled registry, translation and identifier updates; no distinct architectural rule. |
| !9197 | merged | Scanned | Scheduled generated registry/CT metadata refresh; routine maintenance. |
| !9196 | merged | Scanned | Scheduled registry and translation update; no substantive review convention. |
| !9195 | merged | Scanned | Lemon parser grammar formatting consistency without changed language semantics. |
| !9194 | merged | Discussion-focused | Guy asks that renamed Flex pattern names use consistent CamelCase; addressed in !9208. |
| !9193 | merged | Deep | Locamation timestamps: sample pcaps including equal timestamps, divide-by-zero review, TAI presentation, portable guint64 formatting; post-merge Coverity fixed in !9335. |
| !9192 | closed (unmerged) | Discussion-focused | Closed arithmetic-inference draft redirected by author to merged !9212; no accepted implementation. |
| !9191 | merged | Deep | SOME/IP-SD registers only endpoints referenced by genuine SOME/IP services, not non-SOME/IP reserved service 0xfffe. |
| !9190 | merged | Deep | Martin verifies enum domain against bitmask; four-bit DECT mask and guint64 multiply prevent dead path/overflow. |
| !9189 | closed (unmerged) | Discussion-focused | Closed stable-branch feature; Martin requires new capabilities on master; accepted successor !9193. |
| !9188 | merged | Scanned | PDCP-LTE exposes decrypted payload bytes as a filterable field when decryption succeeds. |
| !9187 | merged | Deep | PFCP spec refresh, typed-width checker fixes, and narrowly scoped spare/reserved duplicate-field exceptions. |
| !9186 | merged | Deep | Guy introduces get_executable_path for launching dumpcap/mmdbresolve rather than abusing get_progfile_dir; updates exported symbols. |
| !9185 | merged | Scanned | Guy's extcap Help dialog native-path conversion fix on a maintained release. |
| !9184 | merged | Scanned | Additional stable-branch native-path extcap Help correction. |
| !9183 | merged | Deep | Guy uses QUrl::toLocalFile and QDir::toNativeSeparators; URL path is not a Windows filename. |
| !9182 | merged | Deep | Martin stages typed _add_bits_ checks and repairs field widths/masks; follow-up !9243 restores fuller validation. |
| !9181 | merged | Scanned | Stable extcap installation-root normalization backport. |
| !9180 | closed (unmerged) | Scanned | Closed empty release draft with no code; not an implementation precedent. |
| !9179 | merged | Discussion-focused | Extcap path normalization backport; Guy questions Windows DLL search-path behavior. |
| !9178 | merged | Scanned | Stable PER restricted-character-string offset advance fix. |
| !9177 | merged | Scanned | Additional stable PER parser offset fix. |
| !9176 | merged | Scanned | Sets runtime output directory for log-based extcap executables. |
| !9175 | merged | Deep | Arithmetic expression unary minus grammar and tests; type suitability must also be guarded by !9162. |
| !9174 | merged | Deep | Martin shares E2AP RAN Function mappings by gNB identity across messages, updating ASN.1 source/template and generated dissector. |
| !9173 | merged | Deep | João propagates AST source locations; Guy clarifies they refer to filter-expression text. |
| !9172 | merged | Deep | IPv6 APN6 option: reviewer-required pcap, typed field helpers, value strings, and HBH plus Destination Options. |
| !9171 | closed (unmerged) | Scanned | Closed duplicate IPv6 APN6 proposal; merged successor !9172 is authoritative. |
| !9170 | merged | Scanned | DECT NWK duplicate constant in value_string corrected after author confirmation. |
| !9169 | merged | Deep | Pascal advances PER restricted character string cursor only if length determinant did not already consume input. |
| !9168 | merged | Deep | Gerald fixes extcap-relative resource paths; Guy asks to distinguish physical helper path from app root and recommends backports. |
| !9167 | merged | Scanned | Display-filter arithmetic errors name the unsupported operation. |
| !9166 | open | Discussion-focused | Open TPM reassembly proposal: Martin flags mixed bool conventions, hardcoded ports despite configurable preferences; Alexis rejects fork-master source branch. |
| !9165 | merged | Scanned | Promotes speexdsp to basic Alpine/RPM build dependencies; distro alternatives and setup lists must stay aligned. |
| !9164 | merged | Scanned | BACnet notification new-value option decodes nested DateTime for context tag zero. |
| !9163 | merged | Scanned | NAS 5GS description spelling correction only. |
| !9162 | merged | Deep | Display-filter unary minus checks field-type capability before evaluating; prevents a crash on unsupported types. |

## Durable notebook findings

- Display-filter source spans and grammar diagnostics (!9173, !9202, !9167), unary-minus validation (!9175, !9162), and agreement between inferred constant operands and DFVM storage types (!9207, !9210). Later !9246 partly supersedes arithmetic commutation.
- Source checker coverage and false positives (!9182, !9187), bitmask value-domain verification (!9190), and the later checker completion !9243.
- Extcap main-directory versus physical-executable discovery (!9168, !9179, !9181, !9186), native file URL diagnostics (!9183–!9185), and Windows DLL search-path review.
- Protocol-port registration conditioned on actual SOME/IP service meaning (!9191); logical E2AP RAN state sharing keyed by gNB identity (!9174); TPM preference-versus-hardcoded-port caution from **open** !9166.
- Typed time, TAI presentation, malformed equal timestamps, portable 64-bit output, and post-merge Coverity correction (!9193, !9335).
- Encoding-aware NFSv4 UTF-8 strings (!9205, !9206), PER parser cursor ownership (!9169, !9177, !9178), and visible preference-load errors (!9203).
- Checker- and capture-driven IPv6 APN6 review (!9172), build/registry generator no-op semantics (!9199, !9200), and first-class resource cleanup of GUI error paths (!9211).

**Scope:** this was a repository review/documentation pass. No local builds, packet-capture decoding, runtime tests or fuzzing were performed.

## Continuation

Next expected candidate: **!9161**, subject to re-inventorying the corpus and *exact* review membership on the next run. The recurring task must remain enabled; corpus exhaustion, if encountered, is not a reason to disable it.
