# Automated MR review ledger: !20439–!20488

Corpus revision reviewed: `dheitmueller/wireshark-corpus-mrs@9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, the supplemental automation ledger, and all available per-run files under `reviewed-mrs-automation/`; preserved and counted the historical !17571–!17620 batch. The next candidates were checked against actual corpus entries rather than inferred from numeric ranges. Every MR object !20439 through !20488 exists at the pinned corpus revision and none was already present in the reconstructed reviewed set, so this run reviewed exactly these 50 MRs newest-to-oldest.

Weighting: merged master changes and substantive maintainer discussion receive the most weight; stable-branch backports corroborate their master changes; open, closed, test-only, abandoned, and superseded MRs are recorded but down-weighted. Guy Harris technical feedback is treated as especially authoritative.

## Exact reviewed set (50)

!20488, !20487, !20486, !20485, !20484, !20483, !20482, !20481, !20480, !20479, !20478, !20477, !20476, !20475, !20474, !20473, !20472, !20471, !20470, !20469, !20468, !20467, !20466, !20465, !20464, !20463, !20462, !20461, !20460, !20459, !20458, !20457, !20456, !20455, !20454, !20453, !20452, !20451, !20450, !20449, !20448, !20447, !20446, !20445, !20444, !20443, !20442, !20441, !20440, !20439

## Strongest durable findings

- **!20470 — deep, merged master, Guy Harris review.** SocketCAN/candump interface-ID refactoring prompted Guy to distinguish formats that truly contain per-packet multi-interface identity from formats that do not. He also suggested lifting the reusable interface-mapping mechanism into common `wiretap/wtap.c` infrastructure so iptrace, candump, and other applicable formats can share it. Promoted to `wiretap-interface-metadata-conventions.md`.
- **!20475 — deep, merged master, John Thacker / Anders Broman.** MATE stops mutating parsed configuration during packet processing: counters and indexes move into runtime data and runtime objects retain `const` configuration pointers. This cleanly separates ownership/lifetime and simplifies cleanup. Promoted to `configuration-runtime-state-conventions.md`.
- **!20440 — deep, merged master, John Thacker / Anders Broman.** Pcapng timestamp conversion is algebraically restructured so very fine decimal or binary timestamp resolutions cannot overflow intermediate nanosecond calculations. Promoted to `arithmetic-safety-conventions.md`; the distinct loss-of-precision-on-output issue remains covered by later !20533 guidance.
- **!20464 — deep/corroborating, merged master, Michael Mann.** Large conversion series replaces ambient `wmem_packet_scope()` use with explicit `pinfo->pool` or an explicitly passed allocator. Strong corroboration of the notebook's existing explicit-allocation-context rules; no duplicate rule added.
- **!20458 — discussion-focused, merged master.** Proto display formatting clamps/validates computed `value_pos` and `name_pos` against the destination buffer instead of assuming enough room. Corroborates existing bounded-output/parser-resilience guidance rather than adding a new standalone rule.
- **!20479 — merged master, John Thacker / Anders Broman.** DNP3 removes a local 48-bit timestamp implementation and uses the common `ENC_TIME_MSECS|ENC_LITTLE_ENDIAN` machinery. Useful evidence for preferring canonical field/time decoding APIs over open-coded reconstruction when the core API exactly models the wire format.
- **!20463 → !20472 → !20488 — supersession chain.** The first two Lua STARTTLS submissions were closed and ultimately replaced by merged !20488. Guy Harris's explicit commit-message guidance on !20463 (`component: brief summary`, blank line, details in the body, short first line) is authoritative but already represented in `submission-conventions.md`; the accepted implementation is weighted over the closed predecessors.
- **!20439 — merged stable backport, John Thacker.** DTLS renegotiation decryption avoids treating a transient `SSL_HAVE_SESSION_KEY` state bit as proof that old-session decryption cannot proceed while deriving the replacement key, and removes duplicate null-cipher handling already supported by the shared TLS utility. Useful state-machine/common-helper evidence, but the release backport was not promoted above stronger master-side conventions in this batch.

## Additional reviewed material

The remainder of the batch was scanned for diffs, discussions, disposition, and relationship to accepted successors/backports. It includes protocol correctness and generated-dissector updates (!20486, !20484, !20462, !20460, !20450, !20449, !20447, !20446, !20445, !20444, !20443), platform/build fixes (!20483, !20480, !20466, !20465, !20448, !20441), generated registry/data maintenance (!20487, !20485, !20467, !20455, !20454, !20453, !20451), CI/fuzz artifact maintenance (!20469, !20468, !20456), static-analysis/tooling cleanup (!20473, !20452), parser/format robustness (!20482, !20481, !20461), sharkd option-state correction (!20474), and the DNP3 portability/backport sequence !20476–!20478.

Down-weighted items include closed/superseded STARTTLS submissions !20463 and !20472, pipeline/test-only !20471, closed MCC/MNC attempts !20457 and !20459, and open draft !20442. Their useful review comments were considered, but their implementations are not treated as accepted precedent when a merged successor or stronger evidence exists.

## Notebook changes from this run

- `wiretap-interface-metadata-conventions.md`: added common-layer/per-packet interface-identity guidance from merged !20470 and Guy Harris's review.
- `arithmetic-safety-conventions.md`: added overflow-safe scaling guidance for externally declared timestamp resolutions from merged !20440.
- `configuration-runtime-state-conventions.md`: added the configuration-immutability/runtime-ownership rule from merged !20475.

`reviewed-mrs.md` was intentionally left unchanged; this per-run file is the exact audit ledger for the batch.