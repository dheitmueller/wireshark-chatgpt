# Reviewed Wireshark MRs — automation batch !15399–!15448

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Selection method: rebuilt the already-reviewed set from the available notebook review tracking, including `reviewed-mrs.md`, the aggregate/supplemental automation tracking, and the per-run ledgers under `reviewed-mrs-automation/`. The historical !17571–!17620 batch is explicitly preserved and counted. Selection is by exact MR-number set subtraction against the corpus, not by assuming that a numeric interval is covered because some members appear in a ledger. The prior exact ledger ends with !15449; all fifty selected corpus entries were individually inspected, making the next fifty highest-numbered previously-unreviewed MRs the contiguous set !15448 through !15399.

## Exact reviewed MR set

- !15448, !15447, !15446, !15445, !15444, !15443, !15442, !15441, !15440, !15439
- !15438, !15437, !15436, !15435, !15434, !15433, !15432, !15431, !15430, !15429
- !15428, !15427, !15426, !15425, !15424, !15423, !15422, !15421, !15420, !15419
- !15418, !15417, !15416, !15415, !15414, !15413, !15412, !15411, !15410, !15409
- !15408, !15407, !15406, !15405, !15404, !15403, !15402, !15401, !15400, !15399

Count: **50**.

Merge-status weighting: **48 merged**, **2 closed/unmerged** (!15412 and !15407). Closed proposals were inspected but down-weighted; !15407 was additionally superseded by the generator-side work referenced as !15505.

## Review notes and weighting

- **!15440 — deep, merged master; promoted.** New IBM i TRCCNN RDMA dissector, first contribution, with substantial Martin Mathieson review. Martin asked for specification/sample-capture evidence and the contributor attached `irdma-sample.pcap`. More importantly, Martin challenged putting internal flow/analysis structures in `packet-irdma.h`: only the per-packet data passed to potential subdissectors was genuinely shared, so the remaining analysis structures were moved into `packet-irdma.c` before merge. Added a minimal-header-surface rule to `api-design-conventions.md`.
- **!15430 — deep, merged master; extremely high weight.** Authored and merged by Guy Harris. Linktype 209 historically carried the IPMB name, but its pseudo-header guarantees only I2C and is also used for HDMI DDC-over-I2C. Guy changes Wireshark's name/comment to `I2C_LINUX`, matching the actual substrate semantics rather than the format's original application. Added `encapsulation-naming-conventions.md`.
- **!15405 — deep, merged stable backport; promoted.** sharkd's legacy `fol` result is a positional array-of-arrays that cannot be extended cleanly with stream/sub-stream metadata without breaking clients. The accepted API preserves/deprecates `fol` and adds extensible object-based `followers`, plus a new optional `sub_stream` argument and unit tests. Added `structured-api-evolution-conventions.md`.
- **!15434 — merged master; promoted with !15405.** Adds optional sharkd `interval_units`, defaulting to the historical millisecond interpretation so older requests retain behavior, and adds unit tests. Also aligns sharkd's graph-item limit with the Qt implementation. Added the default-preserves-old-behavior rule to `structured-api-evolution-conventions.md`.
- **!15406 — merged master; promoted.** Authored by John Thacker. TShark's `-l` option promises prompt live output to downstream scripts, but dumpcap batching could still add latency. The accepted change sets dumpcap's update interval to zero when `-l` is used. Extended `capture-diagnostic-conventions.md`: low-latency streaming semantics must propagate through every buffering/IPC stage, not only stdout.
- **!15438 + !15446 + !15448 — deep master plus merged stable backports; strong corroboration.** The WSLua signed-integer path had validated a possibly negative Lua floating number through an unsigned conversion. John Thacker initially reasoned that fixed-width casts would make the result equivalent, then explicitly revised that view after the contributor demonstrated ARM/macOS behavior where floating-to-unsigned conversion can yield zero. This strongly reinforces the existing scripting numeric-conversion rule: reason from the language conversion contract and semantic signedness, not x86 observations. No duplicate notebook section was added.
- **!15407 — closed/unmerged, superseded; down-weighted but useful generated-code evidence.** Gerald Combs pointed out that PIDL-generated dissectors must be fixed at the generator/source level rather than only by editing generated outputs. Anders Broman supplied the `NDR.pm` generator change/regeneration command and then created !15505 for the generator-side solution. This corroborates the existing `generated-code-conventions.md`; no duplicate rule was added.
- **!15402 — merged master; architectural/performance corroboration.** Anders Broman questioned linear lookup cost; Martin Mathieson noted the key fits in 32 bits and a table is possible but the list is short/fixed, recommending merge-as-is and a later lazy table only if worthwhile. Useful evidence against premature lookup complexity; no separate generic rule was promoted.
- **!15441 — merged master; const-correctness corroboration.** Review noted that conversation lookup helpers should take `const packet_info *` where they only inspect it, especially to compose with reassembly APIs already accepting const context. This is already captured by later stronger !15468/!15469 evidence in `const-correctness-conventions.md`.
- **!15413 + !15417 — merged master/stable.** TLS registry labels are corrected to reflect the actual TLS 1.2 SignatureAlgorithm/HashAlgorithm registries rather than importing TLS 1.3 or unrelated algorithm assumptions. Corroborates existing version/registry semantic guidance.
- **!15439 + !15400 — merged master.** Martin Mathieson's typed-item checker fixes correct field masks/encodings. Useful checker evidence, but no novel cross-project rule.
- **!15447, !15436, !15433, !15432, !15431, !15428 — merged master/stable.** Fix `snprintf` sizing/truncation warnings, including use of Wireshark's established item-label bound. Corroborates existing bounded-output/checker guidance.
- **!15422–!15427, !15416, !15418–!15420, !15423, !15424 — merged Qt master/stable fixes.** Flow Graph/TCP graph panning, axis sizing, queued replots, and labels. Accepted UI correctness/performance work but no new durable project-wide rule beyond existing UI conventions.
- **!15421 — merged master, Gerald Combs authored/merged.** Replaces Qt connect-by-name behavior with explicit signal/slot connections to satisfy clazy and make wiring explicit. Useful localized Qt/tooling precedent; not promoted separately.
- **!15445–!15442, !15437, !15435, !15429, !15415, !15414, !15411, !15410, !15409, !15408, !15404, !15403, !15401, !15399 — reviewed.** Automatic data updates, packaging/CI maintenance, protocol-local fixes, stable backports, and small cleanup. Accepted where merged but no additional durable cross-project rule was warranted.
- **!15412 — closed/unmerged; down-weighted.** Abandoned release-4.2 E.212 update attempt; not used as authoritative convention evidence.

## Notebook updates from this run

- `api-design-conventions.md`: added the rule that dissector headers should expose only genuine cross-file/subdissector contracts; internal analysis/state belongs in the `.c` file (!15440). Commit `7ffd652bfeda3595eb20a56b0fa25757edb34b0b`.
- `capture-diagnostic-conventions.md`: added end-to-end low-latency streaming semantics across dumpcap/TShark buffering boundaries (!15406). Commit `751968abc522609fd1cfa7130b497b5545b09787`.
- `structured-api-evolution-conventions.md`: new rules for replacing non-extensible positional JSON shapes without breaking old clients and for optional parameters whose omission preserves historical semantics (!15405, !15434). Commit `424c120d59b77bc5374ae1411ec26b12433ac449`.
- `encapsulation-naming-conventions.md`: new high-authority rule from Guy Harris to name link/encapsulation types for what the format actually guarantees, not merely its original application (!15430). Commit `642b307aa9ffc9327ae4de85638bf5bc3ac2033e`.

## Continuation

`mr_15398.json` exists at the same corpus commit, so the corpus is not exhausted and the next run can continue below this batch.
