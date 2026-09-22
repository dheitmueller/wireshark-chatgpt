# Wireshark MR automation review: !13613-!13662

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `b1d68411dafbadefef503c6d074a18b4967b2d6e`
- Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md` plus all available per-run files under `reviewed-mrs-automation/`, preserving/counting the historical !17571-!17620 batch; then selected the fifty highest-numbered corpus MRs absent from that exact set. The preceding run's !13662 lookup was a frontier probe only and was not counted as reviewed.
- Reviewed count: 50
- Status mix: 49 merged; 1 closed/unmerged (!13639). The closed MR was down-weighted; its CMake-variable proposal was later replaced by !19883.

## Exact reviewed MR numbers

!13662, !13661, !13660, !13659, !13658, !13657, !13656, !13655, !13654, !13653, !13652, !13651, !13650, !13649, !13648, !13647, !13646, !13645, !13644, !13643, !13642, !13641, !13640, !13639, !13638, !13637, !13636, !13635, !13634, !13633, !13632, !13631, !13630, !13629, !13628, !13627, !13626, !13625, !13624, !13623, !13622, !13621, !13620, !13619, !13618, !13617, !13616, !13615, !13614, !13613

## Durable notebook findings promoted

- **!13652 — callback/context pairing.** The UAT file loader invoked the `set` callback with `cbdata.chk` instead of `cbdata.set`, which could corrupt memory when a plugin provided distinct callback context objects. Added `callback-context-conventions.md`: a callback and its callback-specific context pointer are one semantic pair; audit both function and userdata at every invocation. Commit: `0b749376d83da7ada1c819d5259100fb70f0d6fd`.
- **!13632 — stateful decoder plus desegmentation.** HTTP/3 QPACK reassembly was feeding the stateful nghttp3 decoder bytes that had not actually been consumed and would later be presented again after desegmentation. Extended `reassembly-conventions.md`: advance a stateful stream decoder only for the consumed/committed prefix and leave the incomplete suffix for reassembly replay. Commit: `d475f69f2f58431c6fb91bf2b887f4f5c70f15ac`.
- **!13625 with !13626/!13627/!13628 — authoritative wire semantics.** Guy Harris corrected NFLOG timestamps from seconds/nanoseconds to seconds/microseconds using both the LINKTYPE_NFLOG definition and the Linux kernel producer implementation, and added shared `ENC_TIME_SECS_USECS` support rather than local conversion logic. Extended `authoritative-source-preservation-conventions.md`. Commit: `cfae33a5118cc7f0c73f88febc722c1f3b39dfc9`.
- **!13619/!13620 — ABI symbol metadata.** Debian `libwiretap` symbol manifests omitted the real `_value` suffix on newly exported getter/setter names. Extended `abi-compatibility-conventions.md`: package symbol/version manifests must match actual exported API names exactly. Commit: `0ae07de57b46a079ebaec06b5c74ce3e9e03bbdc`.
- **!13624 and !13641 — capture discovery caching and least-work discovery.** John Thacker cached the privileged local-interface query behind explicit refresh/interface-monitor invalidation, and separately made Logray request only extcap interfaces instead of unnecessarily invoking full local capture discovery. Added `capture-discovery-cache-conventions.md`. Commit: `5580828e9569a939e31debeae2da24012e97a65d`.

## Useful corroborating evidence retained without duplicate notebook rules

- **!13633** reinforces the existing single-pass/redissection rule: a stateful QPACK decoder must not be advanced again on revisits; its master change is !13611, which lies beyond this run and was therefore not counted or promoted from the backport alone.
- **!13621/!13622** reinforce that a valid zero-length transport segment does not automatically imply that an application subdissector/desegmentation path should be invoked with zero bytes.
- **!13616** reinforces the existing protocol-field semantic rule: keep a useful raw/numeric protocol value as the filter/TShark value while using display formatting for human-readable interpretation, and keep profile-specific sentinel semantics (Smart Energy zero means “now”) scoped to that profile rather than the generic time helper.
- **!13650** reinforces ownership-by-container design: when a hash table owns dynamically allocated capability values, attaching the value destructor to the table is safer than reproducing manual cleanup at each consumer.
- **!13655** reinforces API lifetime discipline: use a string-copying column API when the supplied string is dynamically allocated rather than assuming static lifetime.
- **!13639** is useful negative/downstream evidence but was not elevated because it closed unmerged and was later superseded. Its discussion nevertheless reinforces that build-system variables consumed by downstream packagers are compatibility surfaces and should be migrated/deprecated deliberately.

## Frontier

`mr_13612.json` exists in the same corpus commit and is merged. It was fetched only to verify that the corpus continues below this batch and is **not** counted as reviewed here. Absent newly scraped higher-numbered unreviewed MRs, !13612 is the next descending candidate.

The corpus was rechecked after the notebook convention commits and remained at `ddcaa22b51c68f594e425a23388c3a2086813054`.
