# Wireshark MR review automation ledger — !19190–!19239

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

This run reviewed exactly 50 previously unreviewed merge requests. Selection was rebuilt from the individual tracking available in `reviewed-mrs.md`, the aggregate automation ledger, and the per-run files under `reviewed-mrs-automation/`; numeric ranges were not treated as reviewed merely from their filenames. The previously reviewed !17571–!17620 batch remains part of the already-reviewed set and was not revisited.

The higher-numbered corpus MRs are already represented in the accumulated tracking, including the backfill/gap ledgers, so this run resumes immediately below the previously reviewed !19240–!19289 batch.

## Exact MRs reviewed

`!19239 !19238 !19237 !19236 !19235 !19234 !19233 !19232 !19231 !19230 !19229 !19228 !19227 !19226 !19225 !19224 !19223 !19222 !19221 !19220 !19219 !19218 !19217 !19216 !19215 !19214 !19213 !19212 !19211 !19210 !19209 !19208 !19207 !19206 !19205 !19204 !19203 !19202 !19201 !19200 !19199 !19198 !19197 !19196 !19195 !19194 !19193 !19192 !19191 !19190`

Count: **50**.

## Review notes

Merged work was weighted above abandoned/superseded work, and maintainer-authored or maintainer-reviewed changes were treated as stronger evidence.

- **!19239** (Martin Mathieson, merged): continued the `tvb_get_bits<N>` API cleanup, replacing unnecessarily wide bit-access helpers with the helper matching the actual extraction width. This corroborates existing API-domain/type-width guidance rather than creating a new rule.
- **!19235** (John Thacker, merged): added optional HTTP MIME sniffing / heuristic-first body dispatch. The accepted design preserves explicit `Content-Type` precedence by default, makes heuristic override opt-in, cites RFC 9110 as part of the default-policy rationale, and saves/restores `pinfo->can_desegment` around heuristic probing. This produced a durable addition to `heuristic-dissector-conventions.md`.
- **!19232** (merged): restricts APRS probing in AX.25 no-layer-3 handling to the UI-frame context required by the protocol, reinforcing that heuristic/subdissector invocation must be gated by the enclosing protocol's structural semantics rather than payload resemblance alone.
- **!19217** (merged): raises the C++ language requirement when Falco/libscap support is enabled because those dependencies require C++17. This reinforces existing build/dependency-contract guidance: optional feature enablement must carry the transitive toolchain requirements of the enabled dependency set.
- **!19199** (merged): corrects ICMP conversation-line behavior for error packets, reinforcing the distinction between the packet carrying an error and the embedded packet/conversation being reported on.
- **!19198** (merged): exports an existing ISUP helper for external plugin use with `WS_DLL_PUBLIC`, corroborating the notebook's ABI/export-surface guidance: declarations intended for external plugins must participate deliberately in the public symbol surface rather than relying on incidental linkage.
- **!19197/!19196** are packaging corrections (license classification and optional RPM dependencies) and provide useful portability evidence but no new source-level convention.
- **!19195/!19194/!19193/!19192** are narrow protocol/documentation/backport corrections and were weighted accordingly.
- **!19191** adds support for Cisco AP-name v2 format in 802.11 vendor-specific data; protocol-specific and not promoted into a general convention.
- **!19190** adds SMB FilePipeLocalInformation/FilePipeRemoteInformation subdissectors; useful protocol coverage but no distinct reusable convention beyond existing subdissector/field-modeling guidance.

The remainder of the batch consists primarily of protocol-specific correctness fixes, API cleanups, documentation/build/packaging maintenance, and closely related follow-up/backport work. Where these reinforced already-recorded conventions, they were not duplicated in the notebook.

## Durable notebook changes

`heuristic-dissector-conventions.md` now records the policy demonstrated by merged !19235: explicit protocol dispatch metadata should retain default precedence; heuristic override of unreliable metadata should be opt-in and specification-informed; and temporary shared dissection-state changes made for heuristic probing must be restored afterward.

Notebook commit for that convention update: `c9727e097c5bb3ed42e5cfa4b144ac4c9d8dfdf4`.

## Continuation

The next run must rebuild the reviewed set from exact ledger contents again. Subject to any individual holes discovered by that reconstruction, the descending frontier is below **!19190**.
