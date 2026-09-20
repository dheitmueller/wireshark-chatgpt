# Automated Wireshark MR review: !16198–!16149

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`
- Reviewed count: 50
- Selection method: enumerate the complete recursive corpus Git tree at the corpus commit (`truncated: false`), reconstruct the already-reviewed MR set from `reviewed-mrs.md` plus every available per-run ledger under `reviewed-mrs-automation/`, explicitly preserve/count the historical !17571–!17620 batch, subtract the reviewed set from the corpus set, sort numerically descending, and take the highest 50. No numeric interval was assumed reviewed merely from neighboring ledger entries.

## Exact reviewed MRs

- !16198
- !16197
- !16196
- !16195
- !16194
- !16193
- !16192
- !16191
- !16190
- !16189
- !16188
- !16187
- !16186
- !16185
- !16184
- !16183
- !16182
- !16181
- !16180
- !16179
- !16178
- !16177
- !16176
- !16175
- !16174
- !16173
- !16172
- !16171
- !16170
- !16169
- !16168
- !16167
- !16166
- !16165
- !16164
- !16163
- !16162
- !16161
- !16160
- !16159
- !16158
- !16157
- !16156
- !16155
- !16154
- !16153
- !16152
- !16151
- !16150
- !16149

## Durable findings promoted to the notebook

### Optional timestamps: absence is not timestamp zero

Merged master !16179, authored and merged by Guy Harris, is strong Wiretap state-modeling evidence. BLF records without an actual timestamp must not be assigned timestamp zero; they should omit `WTAP_HAS_TS`, and derived relative-capture-time state must remain invalid. A downstream path that mishandles a genuinely absent timestamp should be fixed in that downstream layer rather than forcing a sentinel timestamp into the producer's model.

Promoted to `capture-start-time-conventions.md`.

### Externally controlled parser nesting needs an explicit depth bound

Merged master !16197 adds an explicit bounded TLV nesting stack to the RADIUS dictionary parser and terminates parsing with a diagnostic when nesting reaches `MAX_INCLUDE_DEPTH`. Monotonic parser progress is not enough by itself when external input can request arbitrarily many nested levels.

Promoted to `parser-recursion-conventions.md`.

### Non-NULL TVBuff does not imply backing bytes exist

Merged master !16192 fixes a valid-empty decompression path where a non-NULL TVBuff with zero reported length could have no real backing data; testing only the object pointer before `tvb_get_ptr()` caused an assertion. Callers must separately test semantic byte availability when an operation can validly return an empty TVBuff.

Promoted to `tvbuff-parsing-conventions.md`.

### Compiler feature availability includes optional toolchain prerequisites

Merged !16165 initially made MSVC `/Qspectre` unconditional because supported compiler versions recognized the flag. Maintainer review identified that the Spectre-mitigated libraries are optional Visual Studio components. Merged !16173 then reverted the unconditional behavior and restored tested availability handling, explicitly documenting the optional component dependency. The final accepted direction, not the superseded intermediate state, is the precedent.

Promoted to `dependency-capability-detection-conventions.md`.

## Other useful evidence

- !16187 narrows `.gitignore` patterns from `*.so*` to `*.so` and `*.so.*` after the broad pattern hid the legitimate `dictionary.softbank` file. This reinforces the general review preference for narrowly scoped repository-ignore patterns that cannot accidentally hide source/data files.
- !16195 preserves the original SMB2 compressed payload in the packet-bytes UI even after successful decompression, reinforcing the value of keeping original wire bytes inspectable alongside transformed data.
- !16156 and !16155 (stable-branch backports) show that grammar sentinel tokens such as `*` and `]` must be recognized before strict numeric conversion rather than depending on permissive `atoi()` fallback behavior.
- !16153 and !16154 reinforce offset-width and return-value contract rules: parser offsets used in loops must not be stored in tiny wrapping integer types, and a helper expected to return bytes consumed must not return an absolute offset.
- The BLF series !16151, !16159, !16161, !16162, !16163, !16164, and !16166 contains substantial Guy Harris authorship/review. The accepted direction progressively models the actual observed BLF structure, removes stale/unused state, avoids needless zero-fill for fully overwritten buffers, and keeps comments/diagnostics faithful to what the parser actually does. !16159's skip behavior is superseded by !16162's proper support for objects outside log containers and should not be treated as the final architecture.
- !16157 demonstrates routine but useful field-registration review: invalid display-filter characters and vague field labels were caught and fixed before merge.

## Notebook commits

- `be45d67224716514e7a56fbd457527a292f6324d` — absent timestamp semantics from !16179
- `3cc9833341792887e49e9ceb302f17e76de7aca7` — explicit nesting-depth bound from !16197
- `a4215dca818a2485c15cc6679904b3cf58d9e5ad` — valid-empty TVBuff semantics from !16192
- `edf98bb1661fa866bb7bcde253111aa79182fad1` — compiler-option prerequisite detection from !16165/!16173

The corpus continues below this batch; `mr_16148.json` exists at the same corpus commit.