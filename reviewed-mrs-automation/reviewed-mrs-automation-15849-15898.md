# Wireshark MR review automation ledger — !15849–!15898

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook base before this run: `6ef2a93a92d30eaee57f99bb151790edcd002f43`

## Selection

The already-reviewed set was rebuilt from the available review tracking in `reviewed-mrs.md` and the per-run files under `reviewed-mrs-automation/`, including the historical !17571–!17620 batch. Selection was done by exact MR-number set subtraction, not by assuming numeric intervals were complete. The preceding exact ledger covers !16048–!16035 plus !15934–!15899, while earlier ledgers cover the intervening reviewed blocks; no tracking entry marks any MR in !15898–!15849 as reviewed. The fifty highest-numbered remaining corpus MRs are therefore exactly:

`!15898, !15897, !15896, !15895, !15894, !15893, !15892, !15891, !15890, !15889, !15888, !15887, !15886, !15885, !15884, !15883, !15882, !15881, !15880, !15879, !15878, !15877, !15876, !15875, !15874, !15873, !15872, !15871, !15870, !15869, !15868, !15867, !15866, !15865, !15864, !15863, !15862, !15861, !15860, !15859, !15858, !15857, !15856, !15855, !15854, !15853, !15852, !15851, !15850, !15849`.

`mr_15848.json` is present at the same corpus commit, so this run does not exhaust the available corpus.

## Review weighting and durable findings

All fifty MRs above were inspected for state, description, discussions, and the substantive diff where the change/review carried reusable signal. Merged master changes were weighted most heavily; stable-branch backports were treated mainly as corroboration; closed/superseded work was down-weighted. In particular, closed !15889 and !15883 were not used as primary architectural evidence.

Promoted durable conventions:

- **!15887 — MATE: Switch times from floats to doubles (merged, John Thacker; high-authority Guy Harris review).** Guy explains that floating-point equality is inherently fragile for many decimal numerical values and points toward `nstime_t` / `FT_RELATIVE_TIME` as the semantic representation for time values. Added `time-value-field-conventions.md`.
- **!15877 — IPsec NULL heuristic compatibility padding (merged, authored and merged by John Thacker).** Real implementations violate RFC 4303 padding rules, but relaxing padding validation increases heuristic false positives. The accepted design keeps standards-compliant validation as the default and makes looser compatibility an explicit preference. Added this case to `heuristic-dissector-conventions.md`; !15895 and !15896 independently corroborate conservative heuristic defaults and stronger structural discrimination.
- **!15860 — spelling checker pipeline (merged, Martin Mathieson; direct Gerald Combs CI guidance).** Gerald recommends keeping `pyspellchecker` installation local to the one CI job instead of adding it to the shared Ubuntu development image solely for that job. Added `ci-dependency-conventions.md`.
- **!15859 — LLDP: Make dissector less greedy (merged).** Optional End-of-LLDPDU handling exposed both over-consumption of trailing frame bytes and an incorrect consumed-length return. The accepted parser claims only structurally justified bytes by default and leaves ambiguous trailing data available to following dissectors. Added `dissector-consumption-boundary-conventions.md`.
- **!15892 — C15 dissector enhancement (merged after substantial Martin Mathieson review).** Martin explicitly rejects contributor initials embedded in source comments because Git history already records authorship; comments should explain the code rather than its author. Added `source-comment-conventions.md`. The same review also reinforces running commit checks locally and resolving style/filter-name issues before merge.

Strong corroborating evidence not promoted as duplicate rules:

- **!15895** strengthens a pseudowire Ethernet heuristic with captured-length guards plus address, EtherType/length, and SAP validation. It directly corroborates the existing rules that heuristics reject incomplete candidates without throwing and use cheap protocol invariants to reduce false positives.
- **!15896** replaces hard-coded MPLS first-nibble dispatch with dissector-table and heuristic extension points, while leaving heuristic-first behavior disabled by default because of ambiguity and RFC 8469 guidance.
- **!15890** includes a representative NMEA capture with valid and invalid variants plus repeated `fuzz-test.sh` execution, reinforcing the notebook's existing expectation for captures and malformed-input testing.
- **!15881** bounds RTMPT allocation to bytes actually present in the tvbuff while fixing OSS-Fuzz 69219, reinforcing hostile-input allocation/resource-limit guidance.
- **!15863** was iterated with additional captures after the contributor found a query-attribute case the first version did not handle; later review caught narrowing-conversion diagnostics before merge. This reinforces testing multiple protocol variants instead of accepting one happy-path capture.
- **!15861** normalizes Bluetooth SIG UUIDs carried in 32- or 128-bit form to the canonical 16-bit semantic identity so existing specialized dissection and names work, corroborating semantic normalization guidance.
- **!15885** and **!15884**, authored/merged by Guy Harris on maintained release branches, preserve the mergecap protection against using an input file as an append target; !15883 is a closed failed/superseded cherry-pick and carries less evidentiary weight.

The remaining MRs in the batch were protocol-local feature additions, data/table corrections, UI/layout fixes, packaging/build updates, spelling/static-analysis cleanups, or backports whose durable lessons are already represented in the notebook; they were reviewed but did not justify additional notebook rules.

## Notebook commits from this run

- `9efad0aa1b24b1800180c4ebde3a7f3dc23e44fd` — heuristic compatibility relaxations should be opt-in.
- `670c729dee3c95ac5b3a30d8854fe77723a46669` — semantic time-value representation.
- `ec0ec4bd5ddcdd6513d0864028103e7f160b35ae` — CI dependency placement.
- `17ad69ef47e056a1121d777abbae02eb835b24a6` — dissector byte-consumption boundaries.
- `73d6f6233b226d7f514fa94343a33447b9404fab` — source comments should not carry personal authorship tags.
