# Automated Wireshark MR review: !23159-!23208

Corpus source commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to oldest.

Selection method: rebuilt the already-reviewed set from the available notebook tracking, including `reviewed-mrs.md` and the per-run ledgers under `reviewed-mrs-automation/`, and selected the 50 highest-numbered corpus MRs not already represented individually. Numeric ranges were not treated as implicitly complete. The historical !17571-!17620 batch remains explicitly preserved and counted.

Exactly 50 MRs were reviewed in this run:

`!23208 !23207 !23206 !23205 !23204 !23203 !23202 !23201 !23200 !23199`
`!23198 !23197 !23196 !23195 !23194 !23193 !23192 !23191 !23190 !23189`
`!23188 !23187 !23186 !23185 !23184 !23183 !23182 !23181 !23180 !23179`
`!23178 !23177 !23176 !23175 !23174 !23173 !23172 !23171 !23170 !23169`
`!23168 !23167 !23166 !23165 !23164 !23163 !23162 !23161 !23160 !23159`

## Durable findings promoted

- **!23170** (master; John Thacker; merged) and stable backport **!23172**: keep fragmented/truncation-sensitive decoding inside TVBuff-aware APIs so `ReportedBoundsError` retains its reassembly meaning. A failed variable-length decode must not return a sentinel that is then used in offset/progress arithmetic. Added `tvbuff-parsing-conventions.md`.
- **!23171** (merged): a long-running fork-per-client server must explicitly reap completed children. sharkd now drains exited children non-blockingly before forking, preventing zombie accumulation and eventual fork exhaustion. Added `process-lifecycle-conventions.md`.
- **!23191** (master; John Thacker; merged): background/derived GUI dissections must respect the packet-list freeze/configuration lifecycle. Do not compute disposable or potentially incorrect minimap/color state while profiles/preferences are between states and the packet model is intentionally unavailable. Added `ui-dissection-conventions.md`.

## Strong corroborating findings not duplicated

- **!23208**: Jaap Keuter correctly rejected assuming a TCP call contains one complete PDU; the accepted MR removed unsupported TCP handling rather than shipping an unframed TCP dissector. This reinforces the existing TCP PDU/reassembly rule.
- **!23207**: the new typed `proto_tree_add_item_ret_uint8()` / `uint16()` family reinforces the existing fetch-once/type-appropriate return-value guidance rather than requiring a separate rule.
- **!23195/!23194**: further TVBuff unsigned-offset/length migration; counted as corroboration of the established unsigned-domain guidance.
- **!23179**, plus stable backports **!23196/!23197**: bound fixed-capacity packet-derived arrays before writing and keep counts in their true non-negative domain; already covered by hostile-input/bounds guidance.
- **!23173/!23174/!23175**: QUIC reassembly identity must distinguish multiple logical units in the same frame; reinforces existing complete-state/reassembly-key guidance.
- **!23166**: Bluetooth ATT handle-to-UUID state must be keyed by the actual remote device rather than direction alone; strong concrete corroboration of complete protocol-state identity.
- **!23188/!23183**: MaxMind map creation is a required initialization invariant distinct from starting the resolver process. This supports existing lifecycle/state-initialization guidance; the related GUI timing problem is captured more directly by !23191.
- **!23164**: Michael Mann's merged first step toward generic UI preferences, with Guy Harris raising important distinctions among explicit preferences, recent/UI state, profile-dependent settings, and per-conversation settings. Because the MR was explicitly only step 1 and Guy's broader points were architectural questions rather than a fully landed taxonomy, they were recorded here but not over-promoted as settled policy.
- **!23167** was authored/merged by Guy Harris but is a documentation typo fix and carries no durable engineering convention despite the high-authority author/merger.

Release preparation, stable-branch backports, generated-data updates, documentation/asset changes, protocol-specific value corrections, straightforward leak/null fixes, and narrowly local UI changes were reviewed and counted but did not add notebook rules when they provided no new generalizable convention.
