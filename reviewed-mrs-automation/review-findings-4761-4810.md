# Wireshark MR review findings — 4761–4810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly the 50 MRs in the companion exact ledger. Outcome: 49 merged and one closed/unmerged MR (!4788). The closed duplicate is not implementation precedent.

## Strong findings

- **!4810** — display-filter `a != b` becomes the logical negation of `a == b` even for multi-valued fields; the former existential “any unequal” behavior is made explicit as `~=` / `any_ne`.
- **!4807** — Jaap Keuter catches packet-controlled option-length rollover/no-progress risk and pushes for early-continue structure, correct `proto_tree_add_item()` encodings, and `proto_tree_add_item_ret_uint*` where parser logic needs the displayed value. Alexis La Goutte repeatedly requires project pre-commit/checkhf/API cleanup; Jaap later checks Clang analyzer output.
- **!4802** — USB capture-size change with direct Guy Harris review. Guy challenges the platform assumptions and grounds the limit in OS-level USB request/ring-buffer behavior rather than USB bus packet size; he also notes the corresponding libpcap concern. Stable !4803/!4804 corroborate the accepted change.
- **!4801** — USB Attached SCSI dissector. Guy Harris reviews protocol naming; Tomasz Moń requests a sample capture, identifies layer ownership, and catches Visual Studio pointer-cast portability, recommending GLib conversion macros.
- **!4787** — John Thacker replaces an assertion in `tcp_dissect_pdus()` with `FragmentBoundsError` when a subdissector legitimately requests more bytes but TCP reassembly is unavailable.
- **!4786** — CBOR sets the root item length and returns the actual consumed offset rather than the entire captured tvbuff length.
- **!4782** — ORAN section-extension parsing, found by fuzzing local captures, diagnoses reserved zero `extlen` and breaks immediately so the loop cannot re-enter at the same offset. Gerald Combs also documents the master-first stable-backport workflow with `git cherry-pick -x`.
- **!4781 / !4780** — COSE and BPv7 use a subdissector call path whose returned consumed length can distinguish acceptance from rejection, allowing generic CBOR/heuristic fallback.
- **!4778** — John Thacker strengthens DCERPC recognition and documents that a `tcp_dissect_pdus()` length callback result of zero means “length not yet knowable; request more data.” An invalid on-wire zero fragment length must therefore not be returned unchanged.
- **!4769** — Gerald Combs fixes an inverted fuzz-driver conditional. Harness stop/error logic is itself correctness-critical because it decides whether runner/dissector/Valgrind failures are surfaced.
- **!4765** — John Thacker gives `FragmentBoundsError` priority when a tvbuff is known to be an unreassembled fragment; missing reassembly is not necessarily malformed input.
- **!4763** — Alexis La Goutte rejects hard-widening the TZSP channel TLV because it breaks older captures. The merged code decodes using the TLV's actual length and validates both one-byte and two-byte encodings.
- **!4776 / !4777** — restoring `Q_OBJECT` to Qt model classes because translations require it shows that compile success alone does not prove Qt meta-object behavior remains intact.
- **!4773–!4775** — Guy Harris removes wording that tells users to file a known Npcap issue repeatedly; diagnostics should reflect whether a problem is already known and under active investigation.

## Lower-weight / corroborative items

!4809, !4808, !4806, !4805, !4800, !4799, !4798, !4797, !4796, !4795, !4794, !4793, !4792, !4791, !4790, !4789, !4788, !4785, !4784, !4783, !4779, !4772, !4771, !4770, !4768, !4767, !4766, !4764, !4762, and !4761 were reviewed and are primarily backport, automatic-update, narrow cleanup, or corroborative evidence. !4788 is closed because its changes already existed on master.

## Reviewer authority

This batch contains substantive Guy Harris evidence in !4802 and !4801 and a Guy-authored diagnostic-policy change in !4773. Those are weighted highly. John Thacker's merged parser/reassembly changes (!4787, !4778, !4765) are also high-confidence architecture evidence; Jaap Keuter's detailed dissector review in !4807 is strong coding/review guidance.
