# Current Session State

## Active objectives

1. Build a persistent Wireshark engineering notebook that improves future code generation and review quality across conversations.
2. Mine upstream Wireshark merge-request reviews for maintainer conventions and recurring feedback patterns.
3. Apply accumulated findings to ongoing ST 2110-40, ST 2038, VANC/ANC, media-over-IP, testing, and fuzzing work.
4. Prototype generalized Packet Bytes support for non-8-bit word interpretations, initially packed 10-bit ST 291 UDW data.

## Recent development context

- ST 2110-40 dissector development and validation.
- ST 2038 dissector work plus required MPEG PES dispatch integration.
- ST 2110-40 fuzz target brought up with libFuzzer/fuzzshark.
- Independent test-vector search for ST 2110-40 and related standards.
- MR !26390 submitted upstream; Anders Broman review findings have been incorporated into notebook conventions and `personal-review-feedback.md`.
- Current Packet Bytes prototype is in `dheitmueller/wireshark`, branch `djh-10bit`. As of 2026-09-09 the pushed branch head used for this work is `cb138cf01c893dd9d0d0af4eed47926db021e2d4`; refresh the head if Devin pushes again rather than assuming this SHA remains current.

## Packet Bytes 10-bit prototype state

- Goal: allow Packet Bytes to interpret packed data using a word size independently from its numeric presentation. Word interpretation and presentation are separate axes: e.g. 8-bit or 10-bit words versus hexadecimal/decimal/octal/binary presentation.
- The current prototype successfully renders packed ST 291 UDW data as 10-bit words and permits switching presentation independently. It also uses a subset TVB for the packed UDW region so Packet Bytes need not mix 8-bit packet headers and 10-bit payload interpretation in one view.
- ST 291 subdissectors should continue receiving logical 10-bit values represented in 16-bit storage words (valid values 0x000-0x3ff); that internal handoff is distinct from how Packet Bytes receives/displays the raw packed TVB.
- The UI experiment that tries to infer/remember a data-source-wide 10-bit preference from a selected full-source field did not change the desired tab behavior. Do not build further on that inference mechanism.
- Current design direction: make the natural word interpretation/alignment explicit metadata on the Wireshark `data_source` itself. `ST 291 Packed UDW Data` should carry a 10-bit interpretation, bit offset 6, and the logical word count. `DataSourceTab`/`HexDataSourceView` should initialize from that metadata when the tab is created/selected. The normal packet and 8-bit UDW data sources remain 8-bit.
- Presentation remains independently user-controlled; selecting a 10-bit data source should not force hexadecimal specifically.
- Remove/supersede the failed field-to-data-source learn/restore prototype when implementing explicit `data_source` metadata.

## Source/patch workflow for this active branch

- Treat `dheitmueller/wireshark`, branch `djh-10bit`, as authoritative. Refresh/pin its head once at the beginning of a new pushed state.
- For work requiring a repository checkout, shell Git operations, builds, or patch generation, prefer a **fresh ChatGPT Work task** with `Settings -> Data controls -> Work network access -> Allow public internet access` enabled.
- Verified 2026-09-09: the regular-Chat execution sandbox in the existing conversation failed DNS resolution for `gitlab.com`, `github.com`, `example.com`, and `openai.com`; HTTPS and `git ls-remote` therefore also failed. In contrast, a newly created Work task successfully ran `curl -I https://gitlab.com/` and `git ls-remote https://gitlab.com/dheitmueller/wireshark.git refs/heads/djh-10bit`, returning branch head `cb138cf01c893dd9d0d0af4eed47926db021e2d4`.
- In that Work task, `getent hosts gitlab.com` returned no output/status 2 even though `curl` and Git worked. Do not use `getent` alone to decide whether Work has usable network access; test the actual HTTPS/Git operation.
- Connector access, regular-Chat execution networking, and Work cloud networking are separate. A functioning GitLab connector does not imply a regular Chat sandbox can clone the repository.
- Moving or associating an existing Chat with Work should not be assumed to replace its existing execution sandbox with a network-enabled Work environment. Start a fresh Work task when networked repository operations are needed.
- At the start of a Work repository task, run a cheap `git ls-remote` against the authoritative repository/branch. If it succeeds, use a normal clone/fetch/working-tree workflow and stop using connector-to-container reconstruction paths.
- Search results/snippets are not acceptable substitutes for full source files when generating patches.
- Never hand-assemble a patch. Use the real working tree, edit complete files, mechanically generate the diff, and validate it with `git apply --check` against the intended state before delivery.
- If a regular Chat sandbox cannot access the repository, do not spend extended time trying DNS/raw-download/mirror workarounds. Move the editing task to Work or use appropriate connector-side repository actions.
- Keep visible progress updates during lengthy tool operations; ending a response means no work continues in the background.

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

Maintain this as a live checklist of **issues in this specific MR**: correctness bugs, regression risks, mechanical mistakes, style inconsistencies, API misuse, or other concrete cleanup required before submission. Do not use this list for general architecture notes, future ST 2038 work, or broader design ideas unless they expose a regression or defect in this MR. Add newly discovered unresolved issues immediately, and do not consider the replacement MR submission-ready until every item is resolved or explicitly rejected with rationale.

- **ST 2010 reassembly edge cases — unresolved.** Revisit behavior when a new first fragment appears while a previous fragmented message is still active, and when a continuation fragment appears with no active message. Determine from ST 2010 semantics whether these should be diagnosed as malformed/orphan/overlap conditions, and whether the current expert fields/state handling are justified or unnecessarily complex.
- **Combined VANC source cleanup — resolved after human review.** Declaration grouping, major section headers, registration-array formatting, DID/SDID constants, protocol-qualified internal symbols, comment placement, and trivial-helper cleanup were reviewed and corrected.
- **ST 12-2 short payload handling — resolved.** Short payloads create the ST 12-2 protocol item, emit malformed expert information, and return instead of silently returning 0.
- **ST 2016-3 fixed-length handling — resolved.** Any payload length other than 8 bytes emits malformed expert information and returns immediately; redundant later length checks were removed.
- **ST 2016-3 invalid Bar Data flags — resolved.** Mixed/partial top-bottom/left-right flag combinations generate an expert warning; valid combinations are Top+Bottom, Left+Right, or no bars.
- **ST 2016-3 summary text — resolved.** The root tree item includes the human-readable AFD description and 16:9/4:3 mode.
- **ST 291 extension-point comment — resolved.** The comment is transport-neutral and describes `st291.did_sdid` as the extension point for additional native or Lua VANC payload formats.
- **Registration type artifacts — resolved.** Erroneous protocol-specific pseudo-types were corrected to Wireshark's `hf_register_info` / `ei_register_info` types and final files were scanned for recurrence.
- **ST 334-2 short payload handling — resolved.** Payloads shorter than the fixed CDP header create the ST 334-2 tree item, emit malformed expert information, and return instead of silently returning 0.
- **ST 334-2 branch-comment placement — resolved.** Section-specific specification comments sit inside the branches they describe.
- **Trivial WST generated-uint helper — resolved.** Removed `st291_wst_add_generated_uint()` and inlined its call sites.
- **ST 334-2 CDP identifier constant lost during consolidation — resolved.** Restored `#define ST334_2_CDP_IDENTIFIER 0x9669` from the original source.
- **Trivial ST 291 data-byte helper — resolved in final fileset.** Removed `st291_word_data8()` from `packet-smpte-2110-40.c`; DID, SDID/DBN, and data-count extraction now explicitly use `word & 0xff` at each assignment.
- **Public ST 291 API regression check — resolved mechanically.** Final fileset retains the shared header and public helpers including `st291_tree_add_dbn()`; ST 2110-40 includes `packet-smpte-291-vanc.h` rather than duplicating transport-independent logic.
- **Lua extensibility regression check — resolved mechanically.** Final consolidated source still registers the public `st291.did_sdid` dissector table and built-in payload dissectors continue to attach through that table.
- **Compile/test gate — nearly complete.** User reported the corrected pre-final sources build and pass functional testing. The only source change after that test was removal of `st291_word_data8()` and replacement of its three uses with explicit `word & 0xff` assignments. Run one final local rebuild/test against the exact final fileset before submitting the replacement MR.

## Immediate next step

In a fresh network-enabled ChatGPT Work task, clone/fetch `dheitmueller/wireshark`, refresh `djh-10bit`, and implement the Packet Bytes `data_source`-level 10-bit interpretation metadata. Remove the failed field-inference/restore prototype, set explicit metadata on `ST 291 Packed UDW Data`, initialize the Packet Bytes view from it, generate the patch mechanically, and validate applicability before delivery.

## Access state

- `dheitmueller/wireshark-chatgpt`: GitHub write access confirmed and functioning.
- `dheitmueller/wireshark-corpus-mrs`: GitHub read access confirmed; use it instead of GitLab web retrieval for MR archaeology.
- `dheitmueller/wireshark`: GitLab connector read access confirmed. For repository editing/patch generation, prefer a fresh Work task with public network access and a normal Git checkout rather than trying to bridge connector files into a regular-Chat execution sandbox.
