# Wireshark MR review automation: !21591-!21640

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
Exact MRs reviewed in this run: 50

Before selecting this batch, the existing per-run ledgers in `reviewed-mrs-automation/` and the sparse entries in `reviewed-mrs.md` were consulted as the reviewed-set sources. The historical !17571-!17620 batch remains part of the reviewed set. No previously reviewed sparse exception was found inside !21591-!21640, so those 50 highest remaining MR numbers form this run exactly.

Weighting policy: merged master changes and substantive maintainer review carry the most weight; release backports primarily corroborate their master changes; closed/superseded MRs are retained as negative or workflow evidence but are not treated as accepted implementation exemplars.

## Exact reviewed set

- !21640 — **Merged / scanned.** `file-mmodule`: replaces an unnecessary per-entry array allocation with one reusable packet-scope entry object. Useful allocation/lifetime cleanup, but no new convention beyond existing scope/lifetime guidance.
- !21639 — **Merged release-4.6 backport / scanned.** Removes harmless trailing commas after `HFILL`; style cleanup only.
- !21638 — **Merged / deep; very high weight.** Guy Harris authored and merged. Renames `read_record()` to `add_new_record_to_record_list()` because the record is already read, and removes an obsolete boolean return no caller uses. Promoted to `api-design-conventions.md`: helper names and return contracts must track current semantics after refactoring.
- !21637 — **Merged / deep corroboration.** John Thacker LLMNR request/response tracking uses requester identity instead of the ordinary full tuple because requests are multicast and replies unicast. Strong corroboration of the conversation-identity rule already recorded from !21653; no duplicate rule added.
- !21636 — **Merged / scanned.** WSLua `Column_append` gains an optional separator implemented through the existing `col_append_sep_str` helper; straightforward API extension.
- !21635 — **Merged release-4.6 backport / scanned corroboration.** Marks tails after `WS_NORETURN` calls with `ws_assert_not_reached()` for GCC+ASAN while retaining balanced varargs cleanup; reinforces existing compiler/static-analysis portability guidance.
- !21634 — **Merged / scanned.** Removes obsolete Mac OS X 10.6 libpcap workaround; platform-debt cleanup without a distinct new convention.
- !21633 — **Merged / scanned.** Qt/macOS compatibility setting to run without Liquid Glass; platform-specific UI compatibility.
- !21632 — **Merged / scanned.** IANA-IP generated/static data brace correction; no new durable lesson.
- !21631 — **Merged / scanned.** Partial Doxygen documentation for `addr_resolv.h`; documentation-only.
- !21630 — **Merged / scanned.** Doxygen documentation for `ws_assert.h`; documentation-only.
- !21629 — **Closed / discussion-focused; down-weighted.** PLDM GetFirmwareParameters submission. Alexis La Goutte called out `check_dissector.py` warnings (duplicate filter identity, invalid string encoding, and field-width mismatch) and requested squashing. Later merged work covers the feature; retain only as corroboration for running project checks and clean review history.
- !21628 — **Merged / scanned.** Doxygen documentation for `codecs.h`; documentation-only.
- !21627 — **Merged / scanned.** Doxygen documentation for sober128; documentation-only.
- !21626 — **Merged / scanned.** Doxygen documentation for `application_flavor.h`; documentation-only.
- !21625 — **Merged / scanned.** Doxygen documentation for `bits_count_ones.h`; documentation-only.
- !21624 — **Merged / scanned.** Doxygen documentation for `bitswap.h`; documentation-only.
- !21623 — **Merged / scanned.** Doxygen documentation for `cfutils.h`; documentation-only.
- !21622 — **Merged / scanned.** Doxygen documentation for `cmdarg_err.h`; documentation-only.
- !21621 — **Merged release backport / scanned.** Removes obsolete Mac OS X 10.6 dumpcap workaround; same platform-debt cleanup as master counterpart.
- !21620 — **Merged / scanned.** Doxygen documentation for `feature_list.h`; documentation-only.
- !21619 — **Merged / scanned.** Doxygen documentation for `console_win32.h`; documentation-only.
- !21618 — **Merged / deep scan.** Gerald Combs splits system-call capture out of `falcodump` into the separate `dumpcalls` extcap. Good separation-of-responsibility architecture example, but existing notebook layering/modularity guidance already covers the principle.
- !21617 — **Merged / scanned.** Re-enables Falco plugins on macOS CI with targeted handling for known runner issues; CI maintenance, no additional durable rule.
- !21616 — **Merged release-4.6 backport / scanned corroboration.** Backport of the DECT NR cipher-handle error-path cleanup from !21615.
- !21615 — **Merged / deep.** DECT NR closes the gcrypt cipher handle before testing/returning on decryption failure, so successful acquisition is balanced on both success and failure. Strong corroboration of existing cleanup-on-error/resource-lifetime guidance; no duplicate rule added.
- !21614 — **Merged / scanned.** GitLab CI distinguishes plain merge-request pipelines from merge-train pipelines so jobs only run where useful; CI optimization rather than a Wireshark coding convention.
- !21613 — **Merged / scanned.** Pins CMake 3.31 on macOS because CMake 4.1 changed include search behavior undesirably; build-environment compatibility maintenance.
- !21612 — **Merged / deep scan.** CIP routes the embedded-message `proto_item` into common dissection so service text is appended in one shared path rather than only in Multiple Service Request handling. Useful common-path consistency example, already covered by existing shared-implementation guidance.
- !21611 — **Merged / scanned.** Fixes temporary macOS CI CMake workaround; transient CI maintenance.
- !21610 — **Merged / scanned.** Temporary macOS runner CMake update hack; transient CI maintenance.
- !21609 — **Merged release-4.6 backport / scanned corroboration.** Backport of !21607 DECT NR decryption error checking.
- !21608 — **Merged / deep scan.** CIP deliberately exports additional semantic state for downstream dissectors and resets every newly exposed field in `reset_cip_request_info()`. Reinforces explicit state initialization/API ownership; no new notebook rule required.
- !21607 — **Merged / deep.** DECT NR checks cryptographic operation failures instead of assuming successful output, addressing Coverity findings. Corroborates existing rule to validate fallible library operations before using their results.
- !21606 — **Merged / scanned.** Corrects EtherNet/IP Security DTLS timeout units to seconds; protocol correctness fix without broader review guidance.
- !21605 — **Merged / discussion-context scan.** Adds observed MikroTik RoMON destination MAC address while explicitly documenting that IEEE lists it as unassigned. This provides useful evidence that Wireshark may document observed proprietary use without falsely changing registry ownership; related closed !21592 is not treated as an accepted EtherType-registration exemplar.
- !21604 — **Merged release-4.6 backport / scanned.** Backport of GSM SIM AUTHENTICATE Le handling from !21602.
- !21603 — **Merged / scanned.** Header spelling corrections, including Doxygen-visible text; documentation cleanup.
- !21602 — **Merged / deep scan.** GSM SIM AUTHENTICATE consumes optional `Le` only when bytes remain and advances by the short/extended encoded width. Reinforces existing optional-field/offset-consumption rules; no new rule added.
- !21601 — **Merged / scanned.** Doxygen documentation for `crash_info.h`; documentation-only.
- !21600 — **Merged / scanned.** Doxygen documentation for `ws_cpuid.h`; documentation-only.
- !21599 — **Merged / scanned.** Doxygen documentation for `color.h`; documentation-only.
- !21598 — **Merged / scanned.** Doxygen documentation for `cpu_info.h`; documentation-only.
- !21597 — **Merged / scanned.** Doxygen documentation for `version_info.h`; documentation-only.
- !21596 — **Merged / scanned.** Doxygen documentation for `win32-utils.h`; documentation-only.
- !21595 — **Merged / scanned.** Doxygen documentation for `xtea.h`; documentation-only.
- !21594 — **Merged / scanned.** Doxygen documentation for `ws_strptime`; documentation-only.
- !21593 — **Merged release-4.6 backport / scanned.** Adds HTTP/2 `TLS_RENEG_PERMITTED` setting display support; protocol feature/backport with no substantive review discussion.
- !21592 — **Closed / discussion-focused; down-weighted.** Proposed labeling of a RoMON-used EtherType that is registered to another organization. Alexis questioned the ownership/meaning and completeness; John Thacker noted MikroTik's observed unregistered reuse. Closed unmerged, so it is negative/contextual evidence only, not an accepted registry mapping.
- !21591 — **Merged / scanned.** Updates MIDI SysEx manufacturer-ID database and source URL; data maintenance.

## Durable notebook changes from this run

1. **API semantic drift after refactoring — !21638.** Added to `api-design-conventions.md`: when a helper's responsibility changes, re-audit its name and signature. A historical name that describes work no longer performed and a return value no caller uses are both misleading interface debt. This is exceptionally strong evidence because Guy Harris authored and merged the cleanup and stated the semantic rationale directly.

## Strong corroboration retained without duplicate rules

- !21637 reinforces protocol-semantic conversation identity already captured from later LLMNR/TFTP work.
- !21615/!21616 reinforce cleanup of successfully acquired resources on failure paths.
- !21607/!21609 reinforce checking fallible crypto/library operations before consuming output.
- !21602/!21604 reinforce optional-field presence checks and exact offset advancement.
- !21629 reinforces project-specific dissector checks and squashed review-fix history, but is closed/superseded and therefore not implementation precedent.
- !21592 is explicitly down-weighted: the unresolved/closed EtherType labeling proposal is not an accepted registry convention.
