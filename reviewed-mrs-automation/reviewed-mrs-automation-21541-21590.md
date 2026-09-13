# Wireshark MR review automation: !21541-!21590

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
Exact MRs reviewed in this run: 50

Before selecting this batch, all available per-run ledgers in `reviewed-mrs-automation/` and the sparse entries in `reviewed-mrs.md` were consulted as reviewed-set sources. The historical !17571-!17620 batch remains part of the reviewed set. No previously reviewed sparse exception was found inside !21541-!21590, so these are the 50 highest-numbered remaining MRs in the corpus.

Weighting policy: merged master changes and substantive maintainer review carry the most weight; release backports primarily corroborate their master changes; closed/superseded work is down-weighted and used only for discussion/workflow evidence.

## Exact reviewed set

- !21590 — **Merged / scanned.** `wsutil` Doxygen cleanup for `eax.h`; documentation-only.
- !21589 — **Merged / scanned.** `wsutil` Doxygen cleanup for roundup/padding helpers; documentation-only.
- !21588 — **Merged / scanned.** `wsutil` Doxygen cleanup for `wsjson.h`; documentation-only.
- !21587 — **Merged / scanned.** `wsutil` Doxygen cleanup for `wslog.h`; documentation-only.
- !21586 — **Merged / scanned.** `wsutil` Doxygen cleanup for `ws_getopt.h`; documentation-only.
- !21585 — **Merged / scanned.** `wsutil` Doxygen cleanup for `dtoa.h`; documentation-only.
- !21584 — **Merged / scanned.** `wsutil` Doxygen cleanup for `curve25519.h`; documentation-only.
- !21583 — **Merged / scanned.** `wsutil` Doxygen cleanup for `wsgcrypt.h`; documentation-only.
- !21582 — **Merged / scanned.** Removes library installation from `macos-setup.sh` as CI/dependency setup is centralized elsewhere; useful simplification but no new durable convention.
- !21581 — **Merged / scanned.** Doxygen for `ws_mempbrk`; documentation-only.
- !21580 — **Merged / scanned.** Doxygen for `value_string.h`; documentation-only.
- !21579 — **Merged / scanned.** Doxygen for CRC headers; documentation-only.
- !21578 — **Merged release backport / scanned.** XML validation uses libxml2's supported `xmlValidGetPotentialChildren()` API rather than walking structure internals directly, with an explicit bounded result array and warning. Good library-API/bounds corroboration, but backport-only evidence here.
- !21577 — **Merged release backport / scanned.** DECT NR Mode 1 decryption backport; feature propagation, no new review lesson.
- !21576 — **Merged / scanned.** Debian packaging/template wording cleanup; no durable coding rule.
- !21575 — **Merged / scanned.** MIDI SysEx manufacturer database update; data maintenance, with generator work left separate.
- !21574 — **Merged master / deep; very high weight.** John Thacker OSS-Fuzz fix keeps an ASN.1 PER choice index in `uint32_t` instead of narrowing packet-derived unsigned state to signed `int`, avoiding signed-overflow UB. Added as strong parser-specific evidence to `api-domain-conventions.md`.
- !21573 — **Merged release backport / scanned.** NFS directory-delegation support backport; master counterpart carries the primary weight.
- !21572 — **Merged / deep corroboration.** HTTP/2 `TLS_RENEG_PERMITTED` support. Alexis La Goutte explicitly asks for a pcap exercising the option; the contributor supplies one with TLS secrets and identifies the relevant frame. Reinforces representative-capture review guidance.
- !21571 — **Merged / deep corroboration.** Wi-SUN EDFE update accompanied by multiple FAN 1.0/1.1 captures and decryption material, later expanded for multiple sequences. Reinforces testing stateful protocol behavior with representative capture sequences.
- !21570 — **Merged / scanned.** Comment/spelling cleanup; no durable convention.
- !21569 — **Merged / scanned.** Release-note maintenance; no coding convention.
- !21568 — **Merged / deep scan with later static-analysis warning.** DECT NR Mode 1 decryption later attracted Coverity findings including a potential key-index overrun; retained as static-analysis/boundary-check corroboration rather than clean precedent.
- !21567 — **Merged / scanned.** PROFINET segmented-message correction; protocol-specific fix without substantive review discussion.
- !21566 — **Merged / scanned.** Removes trailing commas after `HFILL`; style cleanup. Alexis suggests a check, weakly corroborating automation of mechanical style rules.
- !21565 — **Merged master / deep; extremely high weight.** John Thacker OSS-Fuzz Kafka fix separates a throwing/bounds-checking TVBuff access from a function call whose other arguments contain overflow-prone packet-derived arithmetic. C does not guarantee argument evaluation order, so validation cannot be assumed to happen first. Promoted to `c-expression-sequencing-conventions.md`.
- !21564 — **Merged / scanned.** `packet_info` Doxygen; documentation-only.
- !21563 — **Merged / deep corroboration.** Galileo OSNMA DSM-KROOT support. Post-merge Coverity finds a nullable conversation-state dereference, later fixed in !21669. Reinforces checking nullable state lookups/static-analysis findings; later corrective MR remains the stronger evidence.
- !21562 — **Merged release backport / scanned.** macOS/Windows CI consistency backport; build maintenance.
- !21561 — **Merged release backport / corroboration.** dot11decrypt checks MAC-header length before struct-field access; stable backport of !21544.
- !21560 — **Merged / scanned.** macOS CI dependency setup and Windows hash normalization; CI consistency maintenance.
- !21559 — **Merged release backport / strong corroboration.** SSH `mac_length == -1` sentinel fix; backport of !21558.
- !21558 — **Merged master / deep; very high weight.** John Thacker fixes SSH's `-1` "unknown MAC length" handling. Plain truthiness incorrectly accepts a negative sentinel as a usable length; accepted code tests the positive semantic domain explicitly. Promoted to `api-domain-conventions.md`.
- !21557 — **Merged release backport / scanned.** Windows c-ares dependency update; dependency maintenance.
- !21556 — **Merged master / deep corroboration.** John Thacker OSS-Fuzz BTSDP fix restructures parser arithmetic to prevent signed overflow and handles reported remaining length explicitly. Strong corroboration of established arithmetic-safety/bounds guidance.
- !21555 — **Merged / scanned.** Updates fuzzshark packet-provider initializer after callback-struct growth, preventing missing-field initializer warning; interface maintenance.
- !21554 — **Merged / deep corroboration.** Removes unused `newly_displayed_packets`. In follow-up, Guy Harris explains that an unused `read_record()` boolean should simply disappear if callers do not need it. Strong corroboration of the API semantic-drift rule already promoted from !21638.
- !21553 — **Merged / scanned.** F1AP ASN.1 update to 3GPP v18.7.0; generated/protocol data maintenance.
- !21552 — **Merged / scanned.** Windows c-ares dependency update; dependency maintenance.
- !21551 — **Merged / scanned.** Adds `proto.h` Doxygen; documentation-only.
- !21550 — **Merged / workflow corroboration.** NFS directory-delegation work. Anders Broman explicitly requests squashing to one commit, and the contributor does so. Reinforces existing clean-review-history/submission guidance.
- !21549 — **Merged / scanned.** Deprecates Qt5 in CMake/docs and recommends Qt6; dependency-lifecycle policy, no broader new coding rule.
- !21548 — **Merged release backport / scanned.** Windows Opus dependency update; dependency maintenance.
- !21547 — **Merged release backport / corroboration.** TLS checks `tls13_hkdf_expand_label_context()` before consuming/freing its output. Reinforces the existing rule to check fallible crypto/library operations before use.
- !21546 — **Merged / scanned.** Removes obsolete cast and replaces a hard-coded copy length with `sizeof`; routine type/size safety cleanup.
- !21545 — **Merged release backport / scanned.** Clang 22 compilation fix for invalid uninitialized `const` local array; compiler-compatibility maintenance.
- !21544 — **Merged master / deep corroboration.** dot11decrypt validates MAC-header length before treating bytes as a frame structure. John Thacker also notes longer-term duplication between IEEE 802.11 parsing and crypto-side BSSID extraction; bounds fix is accepted, architecture observation retained as future-refactor context.
- !21543 — **Merged master / strong scan.** Makes pcapng custom-block registration descriptors const because they are read-only; approved and merged by Guy Harris. Useful const-correctness evidence, but no separate notebook rule added.
- !21542 — **Merged / scanned.** Monthly `.mailmap` update; metadata maintenance.
- !21541 — **Merged release backport / scanned.** CQL global-spec dissection correction; protocol-specific backport with no new durable convention.

## Durable notebook changes from this run

1. **Validation must be sequenced explicitly before dependent argument expressions — !21565.** Added to `c-expression-sequencing-conventions.md`. A bounds-checking/throwing accessor in one function argument cannot be relied on to execute before overflow-prone arithmetic in another argument; split the validation into a separate statement.
2. **Preserve packet-derived values in their semantic numeric domain — !21574.** Added as strong evidence to `api-domain-conventions.md`. Do not narrow an unsigned wire choice/index to signed `int` merely for convenience when later arithmetic can overflow the signed domain.
3. **Negative sentinels are not boolean absence/presence flags — !21558/!21559.** Added to `api-domain-conventions.md`. If `-1` means unknown while positive values are actual lengths, test the semantic domain explicitly rather than using C truthiness.

## Strong corroboration retained without duplicate rules

- !21572 and !21571 reinforce the expectation that protocol changes arrive with representative captures, including stateful sequences and required decryption material where applicable.
- !21556 reinforces packet-derived arithmetic restructuring and explicit remaining-length checks under OSS-Fuzz.
- !21547 reinforces checking fallible cryptographic/library operations before consuming their outputs.
- !21554 supplies unusually authoritative Guy Harris corroboration that obsolete return values should be removed rather than carried indefinitely.
- !21550 reinforces squashing review history into a clean logical commit when maintainers request it.
- !21544/!21561 reinforce checking byte availability before interpreting a raw buffer as a fixed-layout structure.
