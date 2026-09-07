# Current Session State

## Active objectives

1. Build a persistent Wireshark engineering notebook that improves future code generation and review quality across conversations.
2. Mine upstream Wireshark merge-request reviews for maintainer conventions and recurring feedback patterns.
3. Apply accumulated findings to ongoing ST 2110-40, ST 2038, VANC/ANC, media-over-IP, testing, and fuzzing work.

## Recent development context

- ST 2110-40 dissector development and validation.
- ST 2038 dissector work plus required MPEG PES dispatch integration.
- ST 2110-40 fuzz target brought up with libFuzzer/fuzzshark.
- Independent test-vector search for ST 2110-40 and related standards.
- MR !26390 submitted upstream; Anders Broman review findings have been incorporated into notebook conventions and `personal-review-feedback.md`.

## MR corpus state

- Raw corpus: `dheitmueller/wireshark-corpus-mrs`.
- Curated notebook: `dheitmueller/wireshark-chatgpt`.
- On 2026-09-07 the complete corpus range !26365 through !26393 (29 MRs) was reviewed/scanned and recorded in `reviewed-mrs.md`.
- Corpus commit `faa3a72bd6a67bf0548e1d646336c6c714ccfa18` added !26206 through !26226; all 21 were reviewed/scanned on 2026-09-07. !26229 was also analyzed because Guy Harris explicitly superseded !26224 with that merged replacement.
- Corpus commit `95ef115dffb8e01e1896fcaebdd09c78764d1712` added a much larger !25933-!26205 batch. Review has **started but is not complete**. MRs actually examined so far are listed individually in `reviewed-mrs.md`; do not mark or assume the full numeric range reviewed.
- Consult `reviewed-mrs.md` rather than assuming a contiguous numeric range is fully reviewed. Continue the 95ef115d batch by selecting unlisted high-information-density/merged MRs first.
- Revisit previously reviewed MRs only when `updated_at`, head SHA, diff versions, discussions, or state have changed.
- Open snapshots worth watching from prior work include !25971 (Tagging Rules architecture), !26374 (RADIUS/RadSec), !26376 (CoAP/Thread), !26386 (Qt turbo navigation), !26390 (ST 2110-40), and !26392 (translation workflow).

## High-value findings from recent corpus passes

- Sample pcaps are strongly expected for protocol/dissector changes; corroborated independently by !22662, !26366, and !26390. Merged !26218 is a good new-dissector submission exemplar with a focused capture, automated tests, expert validation, `check_dissector.py`, and `fuzz-test.sh`. Merged !25977 further demonstrates an explicit MR `Testing` section naming the capture, tshark-visible changed/unaffected behavior, and clean build.
- Prefer `proto_tree_add_item_ret_*` / equivalent return-value tree APIs when a displayed field is also needed by parser logic; !26391 systematically removes double-fetch patterns and !26367 independently demonstrates the idiom. Merged !25946 extends the same principle: reuse already-fetched values and existing `epan/strutil.h` helpers rather than rereading/reinventing.
- Reassembly/length arithmetic needs explicit overflow/capacity handling; !26365, !26382, and the DICOM !26208/!26215 changes provide current examples. Merged !25984 adds a structural check: every used reassembly table must be initialized/registered, and maintainers discussed automating detection of missing registration.
- Differential validation against a mature reference implementation can expose stateful-analysis mistakes that ordinary expected-output tests miss; !26211 compared UDX verdicts packet-by-packet against an instrumented libudx test run.
- Project CI is authoritative when local lint/static-analysis versions differ; !25973 had a Ruff import-order failure in upstream CI that the author's local Ruff did not report.
- Guy Harris rejected runtime guessing of the meaning/type of a dissector `data` pointer in !26224. His merged !26229 replacement uses separate dissector entry points for distinct call contracts with shared parsing code underneath. Treat this as very high-confidence architecture guidance.
- !26223 adds several durable robustness rules: don't hide payload unless structured decoding actually happened; incomplete verification inputs should produce “unverified,” not an invalid verdict; include the complete protocol identity tuple in reassembly keys; validate protocol-specific minimum lengths; and audit display-filter compatibility when changing `hf_` semantics.
- Do not bind a dissector to an unassigned/dynamic UDP port merely because an implementation commonly uses it; !26376 reviewer guidance says to use Decode As (or consider appropriate heuristics separately).
- Prefer consumer-local protocol lookup over adding consumer-specific plumbing to generic TLS/DTLS; !26374 uses `proto_get_id_by_short_name()` in RADIUS after Anders Broman review.
- Hidden protocol-tree items can preserve display-filter fields while another subdissector owns visible dissection; !26393 demonstrates `proto_item_set_hidden()` for eCPRI/O-RAN interaction.
- Wireshark translations are maintained through Transifex rather than direct translation-file MRs (!26392).

## ST 2110-40 replacement MR pre-submission issues

Maintain this as a live checklist. Add newly discovered unresolved issues here immediately, and do not consider the replacement MR submission-ready until every item is resolved or explicitly rejected with rationale.

- **ST 2010 reassembly edge cases — unresolved.** Revisit behavior when a new first fragment appears while a previous fragmented message is still active, and when a continuation fragment appears with no active message. Determine from ST 2010 semantics whether these should be diagnosed as malformed/orphan/overlap conditions, and whether the current expert fields/state handling are justified or unnecessarily complex.
- **Combined VANC source cleanup — in review.** Finish human review of `packet-smpte-291-vanc.c` after consolidation. Current agreed cleanup includes consistent declaration grouping, major section headers, registration-array formatting, named DID/SDID constants, protocol-qualified internal symbols, and removal of trivial helpers that duplicate Wireshark APIs.
- **ST 291 extension-point comment — pending next regeneration.** Use a transport-neutral comment describing `st291.did_sdid` as the public extension point for additional native or Lua VANC payload dissectors; do not describe ST 2110-40/ST 2038 transports as examples of subdissectors.
- **Public ST 291 API preservation — must verify before submission.** Audit `packet-smpte-291-vanc.h` and all callers before removing or changing apparently redundant helpers. In particular, helpers such as `st291_tree_add_dbn()` may be public transport-facing API even when unused inside the combined implementation file. Preserve future ST 2038 compatibility.
- **Lua extensibility — must verify before submission.** Ensure the `st291.did_sdid` dissector table remains public and usable by Lua so non-native VANC payload dissectors such as SCTE-104 or ST 2108 can register by DID/SDID.
- **Replacement-MR organization — planned.** Keep `packet-smpte-2110-40.c` as the transport dissector; keep the transport-independent ST 291 layer and native VANC application dissectors together in `packet-smpte-291-vanc.c`; retain separate protocol registrations/filter namespaces for each VANC standard.
- **Compile/test gate — not yet complete.** After source review, regenerate the final fileset, compile against current Wireshark, run applicable dissector checks/tests and fuzz validation, and compare behavior against representative captures before submission.

## Immediate next step

Continue corpus commit `95ef115d...` from the unreviewed MRs in !25933-!26205, prioritizing merged MRs with substantive human review, especially dissector/libwireshark/reassembly/testing/API work. Update the ledger per MR rather than declaring the whole batch complete until every file has been accounted for.

## Access state

- `dheitmueller/wireshark-chatgpt`: GitHub write access confirmed and functioning.
- `dheitmueller/wireshark-corpus-mrs`: GitHub read access confirmed; use it instead of GitLab web retrieval for MR archaeology.
