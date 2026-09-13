# Wireshark MR review automation: !21391-!21440

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
Exact MRs reviewed in this run: 50

Before selecting this batch, the available per-run ledgers under `reviewed-mrs-automation/` and the sparse tracking in `reviewed-mrs.md` were consulted and combined as the already-reviewed set. The immediately preceding run covers !21441-!21490. No sparse previously-reviewed exception was found among !21391-!21440. The historical !17571-!17620 batch remains preserved and counted. Thus the exact set below is the 50 highest-numbered MRs present at the corpus commit that had not already been reviewed.

Weighting policy: merged master changes and substantive maintainer feedback carry the most weight; release backports primarily corroborate their master changes; closed/superseded work is down-weighted and is not treated as accepted implementation precedent.

## Exact reviewed set

- !21440 — **Merged release backport / scanned.** Darwin droptap drop-reason decoding backport of !21396; substantive licensing and implementation evidence is weighted at the master MR.
- !21439 — **Merged / scanned.** SMB transaction reassembly presentation cleanup; useful implementation cleanup but no distinct durable rule extracted.
- !21438 — **Merged master / strong corroboration.** Mongo OP_MSG validates impossible signed section lengths before length arithmetic can move offsets backward, stall the parser, or overflow. Reinforces malformed-length and parser-progress invariants.
- !21437 — **Merged release backport / scanned.** pcapng Darwin metadata dissection backport; accepted functionality but no separate convention beyond the master work.
- !21436 — **Merged release backport / scanned.** `ws80211` interface-removal correction; platform-specific backport.
- !21435 — **Merged master / discussion-focused.** Debian Stratoshark packaging discussion explicitly considered dependency consequences of moving `falcodump` into `wireshark-common` and deferred the broader package split to keep the current change simpler. Useful scope/dependency tradeoff evidence, but too packaging-specific for a new rule.
- !21434 — **Merged master / scanned.** `ws80211` selects the correct interface to remove; platform-specific correctness fix.
- !21433 — **Merged master / strong review corroboration.** Extcap helpers move to CMake `LIBEXECDIR`. John Thacker checked the derived runtime RPATH contract after the install-location change and identified a follow-up defect; the RPATH correction was handled separately in !21512. Reinforces reviewing dependent runtime assumptions and keeping follow-up fixes scoped.
- !21432 — **Merged master / very strong corroboration.** Makes the c128 include library usable from plain C. Guy Harris directly reviewed the language guards and recommended defining the generic forms only for C11+ or C++, not for unsupported C modes. Strongly corroborates the existing public-header consumer-language validation convention.
- !21431 — **Merged master / review corroboration.** PROFINET Security Credentials OID support. Alexis La Goutte enforced existing file/display-filter naming style and basic cleanliness; no broader new convention.
- !21430 — **Merged master / scanned.** PORTMAP v3/v4 SET/UNSET dissection; straightforward protocol extension.
- !21429 — **Merged release backport / scanned.** Radiotap unknown-TLV display fix backport.
- !21428 — **Merged master / discussion-focused.** TCP Stream graph legend. Review focused on user-visible semantics: default placement should avoid obscuring typical data, menu state should match visible state, and controls must behave consistently in drag/zoom modes. Useful UI review evidence but no new cross-cutting rule promoted.
- !21427 — **Merged master / scanned.** Adds SI-unit toggle to TCP Stream graphs; UI preference/shortcut feature.
- !21426 — **Merged master / scanned.** Automatic data/translation update; no durable convention.
- !21425 — **Merged release update / scanned.** Automatic data/translation update; no durable convention.
- !21424 — **Merged release update / scanned.** Automatic data/translation update; no durable convention.
- !21423 — **Merged release update / scanned.** Automatic data/translation update; no durable convention.
- !21422 — **Merged master / strong corroboration.** `dot11decrypt` moves precise length validation into the decryption functions that own the exact requirements, returns failure on invalid lengths, and validates WEP before decryption. Reinforces entry-point precondition validation and checking before consuming data.
- !21421 — **Merged master / scanned.** Removes an unnecessary temporary MIC copy in dot11decrypt; local simplification.
- !21420 — **Merged master / scanned.** Completes `tvbuff.h` Doxygen coverage; documentation/API maintenance.
- !21419 — **Merged master / scanned.** HTTP/2 notify-URI handling uses the full path instead of one assumed reference-id shape; protocol-specific robustness.
- !21418 — **Merged master / scanned.** Corrects GTPv2-C PGW Change Info IE description; specification alignment.
- !21417 — **Merged release backport / scanned.** EPMD ALIVE2_X response support backport.
- !21416 — **Merged release backport / scanned.** OSPF Link State/TLV identifier corrections backport.
- !21415 — **Merged release backport / corroboration.** kNet undefined-behavior correction backport of !21412.
- !21414 — **Merged release backport / corroboration.** x87 floating-point determinism fix backport of !21411.
- !21413 — **Merged master / scanned.** Narrow GCC warning suppression for `dtoa.c`; build-specific maintenance.
- !21412 — **Merged master / strong corroboration.** John Thacker fixes OSS-Fuzz-detected signed-overflow UB in kNet PDU-length arithmetic by keeping the byte-count/length sum in an unsigned domain. Reinforces semantic type selection and UB avoidance.
- !21411 — **Merged master / deep; promoted.** John Thacker makes `format_units` deterministic on x87/GCC GNU C builds by compiling the affected source with `-fexcess-precision=standard`, rather than allowing 80-bit intermediates to produce architecture/toolchain-dependent boundary results. Promoted to `floating-point-portability-conventions.md`.
- !21410 — **Merged master / scanned.** Adds EPMD ALIVE2_X response support; straightforward protocol extension.
- !21409 — **Merged master / very strong corroboration.** In Gerald Combs's JSON-log/Falco work, Michael Mann challenged a mode boolean threaded through shared state and asked for a separate callable dissector instead. Gerald reverted to that design. Independently corroborates the existing separate-dissector-entry-points/common-parser convention later established even more strongly by Guy Harris in !26224/!26229.
- !21408 — **Merged master / scanned.** OSPF Link State/TLV identifier corrections; registry/specification maintenance.
- !21407 — **Merged master / portability corroboration.** Wiretap replaces BSD `u_char`/`u_int` aliases with standard C99 types where those aliases are not guaranteed by libc feature macros. Consistent with existing portability/type-domain guidance.
- !21406 — **Merged master / scanned.** OSPFv3 Graceful Restart support, reusing existing compatible TLV dissection where appropriate.
- !21405 — **Merged master / scanned.** OSPF Multi-Instance Extensions support and registry alignment.
- !21404 — **Merged master / scanned.** Radiotap unknown non-vendor TLV display/offset correction; protocol parser fix.
- !21403 — **Merged master / scanned.** EtherCAT AMS dissector cleanup; local style/readability cleanup.
- !21402 — **Merged master / scanned.** NSIS removes obsolete quick-launch code; packaging cleanup.
- !21401 — **Merged master / scanned.** Stratoshark release-note update.
- !21400 — **Merged master / corroboration.** Dissector warning fixes; reinforces warnings-as-errors/static-analysis expectations without a distinct new rule.
- !21399 — **Merged master / corroboration.** EtherCAT AMS dead-store fix found by Clang Analyzer; static-analysis cleanup.
- !21398 — **Closed / down-weighted discussion.** Initial broad attempt to replace `proto_tree_add_uint_format` with `_format_value`; Alexis La Goutte questioned whether field-display metadata (`BASE_UINT`/`FT_CUSTOM`) was the better semantic solution. Closed work is retained as review context only; later merged checker/tooling work already provides stronger accepted guidance.
- !21397 — **Merged / scanned.** Stratoshark version bump; release maintenance.
- !21396 — **Merged master / deep; promoted.** Darwin drop-reason mapping could not simply be copied from APSL-licensed XNU into Wireshark. The contributor obtained a separately licensed mapping; Gerald Combs rejected GPL-2.0-only as unsuitable for the project and the final contributed mapping was released under MIT after legal review. The MR also supplied a representative `droptap.pcap`. Promoted as a contribution provenance/license-compatibility convention, without attempting to generalize legal conclusions beyond Wireshark's accepted-license checks.
- !21395 — **Merged / scanned.** Stratoshark release build bookkeeping.
- !21394 — **Merged master / scanned.** Adds capture start/end time to capture-file properties; UI/statistics enhancement.
- !21393 — **Merged / scanned.** Release-note update.
- !21392 — **Merged / scanned.** Wireshark 4.6.0rc1 release build bookkeeping.
- !21391 — **Merged master / scanned.** SIP RFC 3966 `tel:` URI parsing and E.164 handoff; protocol extension with no new cross-cutting convention extracted.

## Durable notebook changes from this run

1. **Constrain excess floating-point precision when exact cross-platform output matters — !21411 (with backport !21414).** Added `floating-point-portability-conventions.md`: when supported compilers/targets are permitted to retain excess intermediate precision that changes boundary results, make the required language semantics explicit at the narrowest practical build scope rather than weakening deterministic regression expectations.

2. **Contribution data/code must have project-acceptable provenance and licensing — !21396 (with backport !21440).** Added `contribution-licensing-conventions.md`: do not copy externally licensed source/data into Wireshark merely because the information is useful; when the originating license is unsuitable, obtain an independently contributed/relicensed representation acceptable to Wireshark and keep the license/provenance explicit. Treat the project's license checker as a submission gate, while escalating actual license questions rather than coding around it.

## Strong corroboration retained without duplicate notebook rules

- !21432 strongly reinforces consumer-language-safe public headers, with direct Guy Harris review.
- !21409 independently reinforces separate dissector entry points for different call contracts rather than mode flags/ambiguous shared state.
- !21438 reinforces malformed-length rejection and parser progress.
- !21422 reinforces precise precondition checks in the function that owns the real operation contract.
- !21412 reinforces unsigned-domain arithmetic for protocol lengths and OSS-Fuzz/UBSan-driven hardening.
- !21433 reinforces auditing dependent runtime assumptions after install-layout changes and handling the discovered RPATH defect as a focused follow-up.
- !21398 is explicitly down-weighted because it was closed and later accepted work provides stronger implementation precedent.
