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
- MR !26390 submitted upstream; Anders Broman review findings have been incorporated into notebook conventions.

## MR corpus state

- Raw corpus: `dheitmueller/wireshark-corpus-mrs`.
- Curated notebook: `dheitmueller/wireshark-chatgpt`.
- On 2026-09-07 the complete corpus range !26365 through !26393 (29 MRs) was reviewed/scanned and recorded in `reviewed-mrs.md`.
- Future corpus mining should start with MRs outside that reviewed range or revisit only open MRs whose `updated_at`, head SHA, diff versions, discussions, or state have changed.
- Open snapshots worth watching: !26374 (RADIUS/RadSec), !26376 (CoAP/Thread), !26386 (Qt turbo navigation), !26390 (ST 2110-40), and !26392 (translation workflow).

## High-value findings from latest corpus pass

- Sample pcaps are strongly expected for protocol/dissector changes; now corroborated independently by !22662, !26366, and !26390.
- Prefer `proto_tree_add_item_ret_*` / equivalent return-value tree APIs when a displayed field is also needed by parser logic; !26391 systematically removes double-fetch patterns and !26367 independently demonstrates the idiom.
- Reassembly arithmetic needs explicit overflow/capacity handling; !26365 and !26382 provide two current patterns.
- Do not bind a dissector to an unassigned/dynamic UDP port merely because an implementation commonly uses it; !26376 reviewer guidance says to use Decode As (or consider appropriate heuristics separately).
- Prefer consumer-local protocol lookup over adding consumer-specific plumbing to generic TLS/DTLS; !26374 uses `proto_get_id_by_short_name()` in RADIUS after Anders Broman review.
- Hidden protocol-tree items can preserve display-filter fields while another subdissector owns visible dissection; !26393 demonstrates `proto_item_set_hidden()` for eCPRI/O-RAN interaction.
- Wireshark translations are maintained through Transifex rather than direct translation-file MRs (!26392).

## Immediate next step

When the corpus repo receives more JSON exports, compare them against `reviewed-mrs.md`, analyze only new/changed high-value MRs, and update the curated notebook automatically.

## Access state

- `dheitmueller/wireshark-chatgpt`: GitHub write access confirmed and functioning.
- `dheitmueller/wireshark-corpus-mrs`: GitHub read access confirmed; use it instead of GitLab web retrieval for MR archaeology.
