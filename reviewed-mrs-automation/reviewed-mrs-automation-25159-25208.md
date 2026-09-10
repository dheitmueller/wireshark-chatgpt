# Wireshark MR review ledger: !25159-!25208

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md`, all per-run ledgers in `reviewed-mrs-automation/`, and the supplemental automation ledger. Selected the fifty highest-numbered corpus MRs absent from that set. The previously reviewed historical batch !17571-!17620 remains counted and preserved. This run reviewed exactly 50 MRs, descending from !25208 through !25159.

## Exact reviewed set

!25208, !25207, !25206, !25205, !25204, !25203, !25202, !25201, !25200, !25199,
!25198, !25197, !25196, !25195, !25194, !25193, !25192, !25191, !25190, !25189,
!25188, !25187, !25186, !25185, !25184, !25183, !25182, !25181, !25180, !25179,
!25178, !25177, !25176, !25175, !25174, !25173, !25172, !25171, !25170, !25169,
!25168, !25167, !25166, !25165, !25164, !25163, !25162, !25161, !25160, !25159.

## Review notes

| MR | Review result |
|---|---|
| !25208 | IEEE1905 zero-address parsing fix. The `continue` path also has to advance per-entry state; reinforces the existing parser-progress rule, so no duplicate notebook rule added. |
| !25207 | CMake minimum-version update. Routine supported-platform/toolchain maintenance; no distinct durable convention beyond existing minimum-capability guidance. |
| !25206 | nl80211 cipher-suite dissection reuses IEEE 802.11 value-string knowledge. Useful shared-definition example, but not strong enough to add a new general rule. |
| !25205 | Qt filter-line-edit affordance rework. UI implementation evolution; no new durable cross-cutting convention extracted. |
| !25204 | STORE DATA `Le` interoperability relaxation/backport. Lower novelty; reinforces pragmatic decoding of real-world noncompliance already recorded. |
| !25203 | SGP.32 object registration. Protocol maintenance/backport; no new general convention. |
| !25202 | PFCP field/name updates. Specification-sync work; no distinct general convention. |
| !25201 | **High-authority merged Guy Harris change.** Do not recreate dependency-owned public typedefs from an assumed private representation; include the authoritative dependency header where its type is exposed, or keep the type private. Promoted to `build-conventions.md`. |
| !25200 | Qt adaptive header sizing. UI behavior fix; no durable architecture rule beyond existing authoritative-state/layout guidance. |
| !25199 | Clang Analyzer dead-store cleanup. Useful static-analysis hygiene, already covered by compiler/checker guidance. |
| !25198 | **Merged John Thacker TLS work.** Computed TLS 1.3 PSK secrets are stored in the normal secret maps so they can flow into pcapng DSBs. Strong corroboration of the shared secrets/DSB lifecycle convention already recorded from later MRs. |
| !25197 | Removal of obsolete Cirrus CI after service shutdown. Reinforces deleting dead CI infrastructure rather than retaining nonfunctional coverage; no new rule needed. |
| !25196 | Debian packaging/LTO/Ninja compatibility work. Packaging-specific variant of build-tool compatibility; no new cross-cutting rule. |
| !25195 | Debian packaging/LTO/Ninja compatibility work. Same accepted direction as related packaging change; no separate rule. |
| !25194 | RTP ED-137 paired-frequency support. Protocol feature addition; no durable general convention extracted. |
| !25193 | PFCP IE expansion. Specification coverage work; no distinct general convention. |
| !25192 | Belden vendor-specific IEEE 802.11 IE dissector. Vendor-extension feature; no durable general convention extracted. |
| !25191 | Qt FontManager single-authority work. Strong architecture example, but later merged theme/font authority work already provides stronger notebook evidence; not duplicated. |
| !25190 | PFCP IE type-list correction. Routine specification synchronization. |
| !25189 | Dissector script-warning fixes. Reinforces repository checker/static-analysis hygiene already recorded. |
| !25188 | 3GPP nettrace UE-ID/IMSI association support. Feature-specific state association; no new cross-cutting rule. |
| !25187 | STORE DATA `Le` interoperability relaxation. Real-world noncompliance handling already covered by existing parser guidance. |
| !25186 | Qt Midnight Blue theme addition. UI content; no general convention. |
| !25185 | **Merged TCP capture-level duplication detection.** The key deliberately normalizes TCP fields expected to change across observation points while retaining payload length and stream scope. Useful semantic-identity example, but too feature-specific to elevate beyond existing complete/stable identity guidance. |
| !25184 | Debian packaging removal of Ninja support under LTO constraints. Build/packaging compatibility evidence; no new durable rule. |
| !25183 | **Merged Guy Harris change.** Uses bounded copying to silence a real string-truncation warning while explicitly documenting that the destination can still truncate semantically. Reinforces that warning cleanup must not obscure remaining correctness limits; existing safety/review guidance is sufficient. |
| !25182 | GeoNetworking ETSI Release 2 PSID support. Specification feature addition; no general convention. |
| !25181 | VoIP label counts/indices made unsigned because counts and array subscripts are nonnegative domains. Reinforces semantic integer-domain guidance already recorded. |
| !25180 | Problematic Unicode removal from generated enterprise data/backport. Data-generation maintenance; no new rule. |
| !25179 | Automatic generated-data update. Low-information maintenance MR. |
| !25178 | Qt 6.10 `[[nodiscard]]` QFile::open handling. Reinforces checking API results rather than discarding them; no separate notebook rule needed. |
| !25177 | weekly-updates.py syntax correction. Tooling bugfix; no new cross-cutting convention. |
| !25176 | Qt FilterEdit/FilterExpressionEdit base-widget introduction. UI factoring work; later UI architecture evidence is stronger. |
| !25175 | Qt profile-dialog fencepost/model-index fix. Reinforces bounds/index correctness already represented elsewhere. |
| !25174 | ETW `SecureZeroMemory` allocation-size correction. Reinforces using the actual allocation extent rather than pointer/object-size confusion; existing buffer-size safety guidance covers it. |
| !25173 | ETW missing type specifier correction. Compiler hygiene; no distinct rule. |
| !25172 | Automatic generated-data update. Low-information maintenance MR. |
| !25171 | Automatic generated-data update. Low-information maintenance MR. |
| !25170 | ETW SMB event-dispatch operator-precedence fix. Reinforces explicit grouping for mixed boolean dispatch conditions; routine correctness rather than a Wireshark-specific convention. |
| !25169 | Closed/superseded Qt theme-token work. Down-weighted relative to subsequently merged theme architecture; not used as authoritative convention evidence. |
| !25168 | DECT NR length-check correction. Reinforces explicit length validation already heavily represented in parser conventions. |
| !25167 | USB Audio AudioStream Interface Control Request support. Protocol feature addition; no new general convention. |
| !25166 | CMake documentation favoring pkgconf over legacy `FindWSWinLibs`. Useful current build-direction signal, but narrower than existing imported-target/dependency-discovery guidance. |
| !25165 | **Merged SMB2 asynchronous-communication support.** Extended transaction identity/state for async IDs, pending/cancel flows and response linkage. Strong state-machine evidence, but existing state/reassembly identity rules already capture the durable lesson. |
| !25164 | Qt welcome-page overlap fix. UI layout correction; no durable general rule. |
| !25163 | Qt Enabled Protocols search dimensions made orthogonal. Good UI-state modeling, already covered more strongly by later single-authority/state-model work. |
| !25162 | Qt theme-preview refresh on System/OS appearance changes. Reinforces derived UI state reacting to authoritative changes; later theme-manager work is stronger evidence. |
| !25161 | Stratoshark theme/default selection. UI theming feature; no new cross-cutting convention. |
| !25160 | **Merged Michael Mann architecture change.** IPv4 shares the complete per-layer `ws_ip4` structure via `proto_data`, and IPv6 lookup supports multiple protocol layers for tunneling. Strong evidence that shared dissection context should preserve complete layer identity rather than assuming one protocol instance per frame; overlaps existing layer/state identity guidance, so not duplicated. |
| !25159 | Personal themes and legacy-color migration. Preference/UI migration feature; no new general convention beyond existing authoritative-state/migration patterns. |

## Durable notebook changes from this run

- Added **External API types belong to the external API** to `build-conventions.md`, based primarily on merged !25201 by Guy Harris and John Thacker review/merge.
- Deliberately did not duplicate existing conventions where this batch supplied corroboration rather than a genuinely new rule, notably parser progress (!25208), shared TLS secret/DSB lifecycle (!25198), semantic integer domains (!25181), complete layer/state identity (!25160/!25165), and single-authority Qt state (!25191 and related theme work).
