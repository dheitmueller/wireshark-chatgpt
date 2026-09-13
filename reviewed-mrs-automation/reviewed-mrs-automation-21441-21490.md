# Wireshark MR review automation: !21441-!21490

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
Exact MRs reviewed in this run: 50

Before selecting this batch, the available per-run ledgers in `reviewed-mrs-automation/` and sparse tracking in `reviewed-mrs.md` were consulted as reviewed-set sources. The immediately preceding automation ledger covers !21491-!21540; no previously reviewed sparse exception was found in !21441-!21490, and the historical !17571-!17620 batch remains preserved and counted. These are therefore the 50 highest-numbered previously unreviewed MRs present in the corpus at the commit above.

Weighting policy: merged master changes and substantive maintainer review carry the most weight; release backports primarily corroborate their master changes; closed/draft/superseded work is down-weighted and is not treated as accepted implementation precedent.

## Exact reviewed set

- !21490 — **Merged / scanned.** CMake libssh 0.11.3 artifact update; dependency maintenance only.
- !21489 — **Merged / scanned.** CMake libgcrypt 1.11.2 artifact update; dependency maintenance only.
- !21488 — **Merged / corroboration.** Parenthesizes macro arguments in `wsutil`; routine macro-safety cleanup, consistent with existing defensive C guidance.
- !21487 — **Merged / scanned.** Refactors common WLAN statistics dialog elements; UI cleanup with no distinct durable convention extracted.
- !21486 — **Merged / strong corroboration.** TLS checks `tls13_hkdf_expand_label_context()` before consuming its output, reinforcing the existing rule to check fallible crypto/helper operations before using their results.
- !21485 — **Merged master / deep corroboration.** Michael Mann moves SAMR-specific obsolete-preference registration out of generic DCE/RPC runtime dispatch and into generated SAMR registration in `NDR.pm`. Reinforces ownership locality and putting generated protocol-specific behavior at its owning protocol rather than making every generic instance branch on identity.
- !21484 — **Merged / corroboration.** IAX2 validates address type before using an API with stronger address assumptions; static-analysis-driven API precondition checking.
- !21483 — **Merged / corroboration.** XML checks for NULL before `g_str_equal`; static-analysis-driven contract validation.
- !21482 — **Merged master / deep; promoted.** Adds a mechanical packaging check that shipped public headers can be included/preprocessed from a pure-C consumer context after a shipped header failed that contract. Test input is generated from the authoritative header set rather than maintained as a parallel list. Guy Harris was reviewer; Anders Broman merged. Promoted to `public-header-validation-conventions.md`.
- !21481 — **Merged / scanned.** Moves macOS/Windows releases to Qt 6.9.2; dependency lifecycle maintenance.
- !21480 — **Merged / scanned.** WSDG Visual Studio reference updates; documentation maintenance.
- !21479 — **Merged master / deep corroboration.** Moves large Catapult DCT2000 mutable line buffers from process-wide statics into per-parser private state, keeping only a small probe buffer on the stack; explicitly reduces `.bss` and avoids conflicts between concurrent parser instances. Strongly corroborates `reentrancy-conventions.md`.
- !21478 — **Merged / scanned.** Documentation backport/update for Visual Studio references; no new convention.
- !21477 — **Merged / scanned.** RPM `%pre` packaging correction; packaging-specific maintenance.
- !21476 — **Merged / scanned.** RPM libexecdir packaging follow-up; packaging-specific maintenance.
- !21475 — **Merged master / deep corroboration.** Clarifies ownership of heap strings returned by `ex_opt_get_next()` so callers can and do free them. The MR itself was re-issued after the author accidentally clobbered the earlier branch, also corroborating clean topic-branch discipline. Michael Mann questioned destructive option retrieval semantics; no new rule beyond existing ownership/API and submission guidance.
- !21474 — **Merged release backport / scanned.** TLS delegated-credentials client-authentication fix; same accepted logic as master change.
- !21473 — **Merged release backport / scanned.** TLS delegated-credentials client-authentication fix; same accepted logic as master change.
- !21472 — **Merged / corroboration.** Frees SMB transaction-fragment tracking hash state; routine resource-lifetime cleanup.
- !21471 — **Merged / scanned.** RPM `%pre` packaging correction; no general notebook addition.
- !21470 — **Merged / scanned.** RPM libexecdir packaging correction; no general notebook addition.
- !21469 — **Merged / corroboration.** XML NULL check prompted by scan-build; reinforces API precondition checking.
- !21468 — **Merged / scanned.** Debian Stratoshark package includes `/usr/bin/strato`; packaging maintenance.
- !21467 — **Merged / corroboration.** Broad dissector-warning cleanup; reinforces existing warning/static-analysis practices without a distinct new rule.
- !21466 — **Merged master / medium.** Corrects TLS delegated-credentials handling for client authentication; protocol-specific correctness fix, with stable backports above.
- !21465 — **Merged master / deep corroboration.** Large ASTERIX rewrite separates generated and manual implementation. Jaap Keuter flagged that generated output exposed too many variables globally; the author corrected them to module-static. Reinforces existing internal-linkage discipline and the rule that generated code must obey the same architectural visibility constraints as hand-written code.
- !21464 — **Merged master / scanned.** Installs extcap helpers under CMake `LIBEXECDIR`; build/install-layout maintenance.
- !21463 — **Merged / scanned.** TCP stream dialog restricts drag/zoom gestures to left mouse button; UI behavior fix.
- !21462 — **Merged / scanned.** WSDG Qt-version documentation update.
- !21461 — **Merged / scanned.** QCPErrorBar legend paint clipping fix; UI detail only.
- !21460 — **Merged release backport / scanned.** Xylan attribute addition; protocol-data maintenance.
- !21459 — **Merged / scanned.** WSDG Qt-version documentation update.
- !21458 — **Merged / scanned.** Qt filter-button popup transient-parent fix; UI-specific correctness.
- !21457 — **Merged master / scanned.** Xylan attribute addition; no broad convention extracted.
- !21456 — **Merged release backport / corroboration.** Darwin Service Class Code field widened to match its value domain; corroborates field-domain consistency guidance.
- !21455 — **Merged master / corroboration.** Refactors DHCPv6/DNS to share SVCB parameter parsing rather than duplicate protocol-equivalent parsing; reinforces shared-implementation/lowest-common-owner guidance.
- !21454 — **Merged / scanned.** Qt filter-button popup transient-parent fix; UI-specific correctness.
- !21453 — **Merged / corroboration.** RTPS NULL checks prompted by scan-build; routine static-analysis hardening.
- !21452 — **Merged / scanned.** Release-note update for macOS/Darwin process information.
- !21451 — **Merged master / strong corroboration.** AMS dissector cleanup uses semantic helpers such as `proto_tree_add_item_ret_uint()` and `proto_tree_add_bitmask()`, removes unnecessary tree guards, and simplifies value handling. Reinforces existing semantic-helper and avoid-double-fetch guidance.
- !21450 — **Merged / corroboration.** Removes Clang Analyzer dead store in AMS plugin; static-analysis cleanup.
- !21449 — **Closed / down-weighted.** Attempt to expand JSON-log support for Kubernetes/Google Cloud/CloudTrail as Falco Events. Closed work is retained only as context and not treated as accepted architecture precedent.
- !21448 — **Merged release/backport / corroboration.** Mongo OP_MSG malformed section-length handling avoids an infinite-loop/no-progress condition; corroborates parser-progress and malformed-length guidance.
- !21447 — **Merged release/backport / corroboration.** Same Mongo malformed section-length/infinite-loop correction; no additional rule.
- !21446 — **Merged master / strong corroboration.** Mongo rejects/handles invalid OP_MSG section lengths so offset advancement cannot collapse to zero and loop forever. Reinforces explicit malformed-length validation and parser progress invariants.
- !21445 — **Merged / strong corroboration.** Makes the c128 public include library usable from plain C by guarding C++ templates with `__cplusplus`; direct precursor/evidence for the public-header validation rule promoted from !21482.
- !21444 — **Merged / scanned.** Corrects CIGI4 acceleration packet label descriptions; presentation fix only.
- !21443 — **Merged master / strong corroboration.** Changes a bit-shift base to unsigned (`1U`) so shifting into bit 31 is defined; GCC UBSan surfaced the problem. Reinforces arithmetic/undefined-behavior guidance and sanitizer value.
- !21442 — **Merged master / corroboration.** Darwin Service Class Code field widened to match its represented value domain; field-registration consistency evidence.
- !21441 — **Merged master / medium.** HTTP2/GTPv2 handover uses the correct SM-context data during 5G→4G handover. Protocol-specific context/state fix; no additional general rule extracted.

## Durable notebook changes from this run

1. **Test shipped public headers from a clean consumer context — !21482, corroborated by !21445/!21495.** Added `public-header-validation-conventions.md`: installed headers should be mechanically included/preprocessed in supported consumer language modes, preferably with the test generated from the authoritative shipped-header set so new headers are covered automatically.

## Strong corroboration retained without duplicate rules

- !21479 independently reinforces storing mutable parser state at per-instance lifetime rather than in process-wide static buffers.
- !21485 reinforces protocol-specific ownership locality and generating protocol-owned behavior at its source rather than branching generically at runtime.
- !21465 reinforces internal linkage for generated symbols unless external linkage is intentionally part of an interface.
- !21486 reinforces checking fallible cryptographic/helper calls before consuming output.
- !21446/!21448/!21447 reinforce that malformed lengths must not allow a parser loop to make zero or backward progress.
- !21451 reinforces semantic proto-tree helpers and avoiding redundant extraction.
- !21443 reinforces unsigned-domain correctness for high-bit shifts and using UBSan/static tooling to expose undefined behavior.
