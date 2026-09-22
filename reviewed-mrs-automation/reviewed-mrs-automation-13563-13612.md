# Wireshark MR automation review: !13563-!13612

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `e1c90c3f4a0fdd7628c705bc3c7ab87328f22047`
- Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md` plus all available per-run files under `reviewed-mrs-automation/`, counting only explicitly listed MR numbers and preserving/counting the historical !17571-!17620 batch; then selected the fifty highest-numbered corpus MRs absent from that exact set. The preceding run's !13612 lookup was a frontier probe only and was not counted as reviewed.
- Reviewed count: 50
- Status mix: 48 merged; 2 closed/unmerged (!13606 and !13587). Both closed Thread/TREL proposals were down-weighted; they were large, failed/reworked submissions rather than accepted architecture.

## Exact reviewed MR numbers

!13612, !13611, !13610, !13609, !13608, !13607, !13606, !13605, !13604, !13603, !13602, !13601, !13600, !13599, !13598, !13597, !13596, !13595, !13594, !13593, !13592, !13591, !13590, !13589, !13588, !13587, !13586, !13585, !13584, !13583, !13582, !13581, !13580, !13579, !13578, !13577, !13576, !13575, !13574, !13573, !13572, !13571, !13570, !13569, !13568, !13567, !13566, !13565, !13564, !13563

Only the MR numbers in the line above are counted as reviewed by this run.

## Durable notebook findings promoted

- **!13585, !13588, and !13589 — honor dependency API failure contracts.** Three adjacent master changes authored by Guy Harris check libpcap error returns even where failure is unlikely or current call ordering appears to make the documented error unreachable. Extended `api-error-reporting-conventions.md`: do not discard an external API's status merely because today's invariants make failure seem impossible; preserve useful root-cause diagnostics at the failing call. Commit: `7438c9f1d9c8c94d9689e4eba1fbf2ce5fcb6c40`.
- **!13602 — classify explicit capture sources before device discovery.** John Thacker recognizes Windows named-pipe syntax without retrieving the interface list and centralizes the previously duplicated test used by `capture_opts`, `dumpcap`, and `rawshark`. Extended `capture-discovery-cache-conventions.md`: special source classes should bypass unrelated expensive/privileged enumeration, and shared entry points should use one classifier. Commit: `52a948c923fc62f6c4fb7d5c9082920dd64d1f75`.
- **!13599 with !13574/!13576 — source checkers need semantic context and narrow exceptions.** Martin Mathieson's typed-item checker requires non-zero masks specifically for fields used in bitmask field arrays and avoids suggesting an invalid zero-mask fix there; adjacent review distinguishes a legitimate non-contiguous RDP mask and a harmless leading zero from a genuinely out-of-width mask. Added `source-checker-conventions.md`. Commit: `16c8bd99bb1e568a69715dd96d5a96a024fb2677`.
- **!13582 with !13572/!13573/!13571 — feature-disabled builds are real configurations.** Guy Harris explicitly caught plugin code/UI that must disappear when plugins are not built; John Thacker's no-libpcap fix disables individual capture actions so absent capture support cannot leave invalid UI/shutdown state; the No Options CI job deliberately turns off normally enabled features to catch exactly these paths. Added this to `build-variant-linkage-conventions.md`. Commit: `5efbda9111a1455c92f3250dcf89b04226f9fff1`.
- **!13570 — static-library consumers own transitive link requirements.** Guy Harris's authored-and-merged FreeBSD fix shows that pulling JSON-dumper code from statically linked `libwsutil` introduces an `isfinite()` dependency that requires explicit `libm` linkage even though macOS does not. The same new build notebook file records the portable link-model rule. Commit: `5efbda9111a1455c92f3250dcf89b04226f9fff1`.
- **!13611 with !13565 and prior !13632 — stateful stream decoders must advance exactly once.** John Thacker's QPACK series separates first-pass decoder state advancement from redissection, uses tvbuff bounds handling rather than unsafe pointer arithmetic at incomplete boundaries, and ensures desegmentation does not replay bytes that were already committed. Added `stateful-stream-decoder-conventions.md`. Commit: `3462d01c18f79f411d04c7263b4aca0b17d6c79e`.

## Useful corroborating evidence retained without duplicate notebook rules

- **!13580/!13592** reinforce the existing default-versus-current-state rule: heuristic dissectors retain an immutable `enabled_by_default` value separately from mutable profile state so reset and `tshark -G` default reporting do not depend on current mutations.
- **!13586** contains direct Gerald Combs guidance that Wireshark source files are UTF-8 and repository scripts processing source should read them as UTF-8; the accepted checker also avoids crashing on incidental non-source files such as editor swap files.
- **!13567/!13568**, authored by Guy Harris, reinforce portability review beyond the platform that first supplied a patch: the FreeBSD `strptime` workaround was generalized so it did not assume a locale was on standard time at the Unix epoch.
- **!13569** is a broad mechanical cleanup confirming that registered tap IDs need not use `-1` initialization; zero-initialized static storage is valid because a tap ID is never zero.
- **!13593/!13595** reinforce defensive Qt selection handling: `selectedRows()` can legitimately be empty and must be checked before indexing.
- **!13606 and !13587** were closed/unmerged Thread/TREL submissions and were down-weighted. Their large scope, failed/reworked state, and replacement workflow are useful submission-history evidence but not strong enough to establish architecture conventions.

## Frontier

`mr_13562.json` exists in the same corpus commit and is a closed draft. It was fetched only to verify that the corpus continues below this batch and is **not** counted as reviewed here. Absent newly scraped higher-numbered unreviewed MRs, !13562 is the next descending candidate.

The corpus was rechecked after the notebook convention commits and remained at `ddcaa22b51c68f594e425a23388c3a2086813054`.