# Review findings: !8611–!8660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed corpus MRs, descending from !8660 through !8611. Forty-nine merged; !8641 closed without merge. Merged master implementations were weighted most heavily, stable backports mainly as corroboration, and the closed !8641 below its merged successor !8642. The historical !17571–!17620 batch remains preserved and counted as 50 unique reviewed MRs.

## Durable findings promoted

- **!8660 — deep, merged master, João Valverde.** Raw packet bytes and the decoded typed field are different semantic value domains. The accepted display-filter `@field` syntax returns `FT_BYTES` from the field's packet-backed range while ordinary field references retain their registered type. Raw-ness is carried explicitly through compiler/VM/reference caching and covered by docs/tests. Added to `dfilter-conventions.md`.
- **!8645, with !8654/!8655 backports — deep.** PER open-type length is in bytes while `tvb_new_octet_aligned()` consumes bits. Convert explicitly at the API boundary; integer type/name alone does not communicate units. Added to `unit-contract-conventions.md`.
- **!8638 — deep, merged master, John Thacker; !8658 backport.** SCTP Host Name is counted and NUL-terminated, so model it as `FT_STRINGZ`; reuse the tree API's returned display string instead of independently formatting it. Do not use the source octet count as a width for converted UTF-8 because replacement/escaping can change output length. Added with !8623 to `text-encoding-conventions.md`.
- **!8623 — deep, merged master, John Thacker.** A wire-defined byte reversal must happen before decoding. Reversing an already-decoded string bytewise corrupts multibyte replacement characters. Added to `text-encoding-conventions.md`.
- **!8622 — deep, merged master, approved by João Valverde.** Values that survive Wireshark `TRY`/`CATCH` non-local jumps must obey the C `setjmp`/`longjmp` storage rules; the accepted test fix moves exception/result state out of the block and marks the cross-jump locals `volatile`. Added to `exception-boundary-conventions.md`.
- **!8618/!8619 — deep, merged, Gerald Combs.** Expensive Linux package jobs whose cache behavior makes per-merge execution wasteful can retain coverage on an explicit recurring schedule. Added to `ci-resource-scheduling-conventions.md`.
- **!8616 — deep, merged master, João Valverde.** Column APIs distinguish not only lifetime but display-safety semantics: `col_set_str()` is for safe static non-formatted strings, while packet-derived text should use the copying/formatting path that validates and escapes display text. Added to `column-string-conventions.md`.

## Important corroboration and review evidence

- **!8659 and !8651 — merged stable backports.** IP fragment offset had the registered mask applied after the value was already manually normalized. Passing the raw field value to the registered-field API and formatting only the displayed derived byte offset reinforces the existing “registered mask applies exactly once” rule in `field-decoding-api-conventions.md`.
- **!8643 and !8644, with !8646/!8647 backports — merged, Gerald Combs.** Repeated `snprintf` appends with a manually maintained offset are fragile because every iteration must preserve both start offset and remaining capacity. `wmem_strbuf` is the preferred construction primitive for variable repeated appends. This corroborates the later, stronger string-builder guidance already in `string-api-conventions.md`.
- **!8634 — merged master, John Thacker.** GTP/GTPv2 FQDN fields use their actual DNS-label/APN encoding via `ENC_APN_STR` rather than hand-mutating an ASCII buffer into dotted form. This corroborates explicit wire-encoding context and the text rule promoted above.
- **!8650 — merged.** Alexis La Goutte requested a capture and flagged analyzer warnings while reviewing the IEEE 802.15.4 TAP PHY Header addition; the contributor supplied focused PHR captures, split unrelated dissector changes, and fixed the warnings. Strong corroboration of existing capture-based submission and static-analysis practice.
- **!8649 — merged master.** S7Comm invalid-character display is routed through a reusable character-value formatting helper; John Thacker requested a semantically broader helper name (`char_val_to_str`) and the author adopted it.
- **!8631 and !8613 — merged.** Clang Analyzer dead-code/dead-store findings produced direct source cleanup across multiple dissectors, reinforcing that analyzer output should feed normal source hygiene rather than be ignored.
- **!8611 — merged release backport, authored by Guy Harris.** Raw-octet ASCII classification uses the 0x80 bit, with Guy documenting why this is independent of whether plain `char` is signed on a platform. Useful high-authority portability evidence, but a narrow backport and not promoted as a separate notebook rule.

## Exact 50-MR review inventory

| MR | Outcome | Review result |
|---|---|---|
| !8660 | merged | Deep; raw-byte display-filter semantics promoted. |
| !8659 | merged | Stable backport; IP registered-mask/double-transform corroboration. |
| !8658 | merged | Stable backport of SCTP host-name handling; corroborates !8638. |
| !8657 | merged | Stable naming backport; no new rule. |
| !8656 | merged | H.248 naming plus synchronized generated source/template; generated-source discipline corroboration. |
| !8655 | merged | Stable PER byte-to-bit unit backport; corroborates !8645. |
| !8654 | merged | Stable PER byte-to-bit unit backport; corroborates !8645. |
| !8653 | merged | Python tool cleanup; no durable project rule. |
| !8652 | merged | Windows workflow dependency-source update; routine CI maintenance. |
| !8651 | merged | Stable IP registered-mask/double-transform corroboration. |
| !8650 | merged | Discussion-focused; focused pcap + analyzer-fix submission evidence. |
| !8649 | merged | Text/value formatting helper cleanup with John Thacker naming review. |
| !8648 | merged | TCP TARR draft revision; protocol-specific. |
| !8647 | merged | Stable DSCP string-builder backport. |
| !8646 | merged | Stable DSCP string-builder backport. |
| !8645 | merged | Deep; PER byte/bit unit contract promoted. |
| !8644 | merged | DSCP `wmem_strbuf` master fix; corroboration. |
| !8643 | merged | SCIDX repeated-append `wmem_strbuf` master fix; corroboration. |
| !8642 | merged | GitLab CI variables represented as strings for local-runner compatibility. |
| !8641 | closed | Down-weighted duplicate/predecessor of merged !8642. |
| !8640 | merged | H.224 label typo; no broader rule. |
| !8639 | merged | Thrift field-abbreviation correction; narrow naming fix. |
| !8638 | merged | Deep; SCTP string representation/decoded-width rule promoted. |
| !8637 | merged | Guy Harris-authored H.224 comment cleanup; no broader rule. |
| !8636 | merged | H.xxx protocol display naming; narrow presentation cleanup. |
| !8635 | merged | Correct printf integer-format macro for hexadecimal output. |
| !8634 | merged | Encoding-aware GTP FQDN decoding; strong corroboration. |
| !8633 | merged | Windows workflow dependency-source update; routine CI maintenance. |
| !8632 | merged | Bluetooth Volume Control Service decoding; reviewer naming/typo cleanup only. |
| !8631 | merged | Clang Analyzer dead-code cleanup; corroboration. |
| !8630 | merged | Automatic data/translation update; no new rule. |
| !8629 | merged | Automatic data update; no new rule. |
| !8628 | merged | Automatic data/release update; no new rule. |
| !8627 | merged | BLF SRR/RTR stable backport; protocol-specific. |
| !8626 | merged | BLF SRR/RTR stable backport; protocol-specific. |
| !8625 | merged | X2AP private-IE ASN.1 configuration/generated output; generated-source corroboration. |
| !8624 | merged | Qt Resolved Addresses condition fix; narrow UI correctness. |
| !8623 | merged | Deep; reverse encoded bytes before text decoding; promoted. |
| !8622 | merged | Deep; non-local-jump/compiler portability promoted. |
| !8621 | merged | Win64 packaging environment fix; routine CI. |
| !8620 | merged | Removes redundant Latest Clang job because other CI already covers it. |
| !8619 | merged | RPM tests moved to scheduled cadence; promoted with !8618. |
| !8618 | merged | Deep; expensive Linux packaging moved to scheduled cadence; promoted. |
| !8617 | merged | GitHub Actions YAML consistency cleanup. |
| !8616 | merged | Deep; column display-string safety contract promoted. |
| !8615 | merged | GitHub Actions YAML consistency cleanup. |
| !8614 | merged | ASCII language code through encoding-aware TVBuff API; existing text rule corroboration. |
| !8613 | merged | Clang Analyzer dead-store cleanup; corroboration. |
| !8612 | merged | H.224 protocol filter-name convention correction. |
| !8611 | merged | Guy Harris-authored stable ASCII-octet portability backport; qualified corroboration. |

## Frontier

!8610 exists in the same corpus snapshot and is merged. It was inspected only as the next-frontier probe and is not counted in this run.
