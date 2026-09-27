# Review findings: Wireshark MRs !7161–!7210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs were merged. H = durable cross-cutting evidence, M = useful accepted evidence, L = local/maintenance.

| MR | Weight | Finding |
|---|---|---|
| !7210 | L | DCT2000 R16 LTE/NR dispatch update. |
| !7209 | H | Keep source-model column identities stable; hide presentation columns in a proxy. |
| !7208 | M | Capture-options fix keeps file duration, file interval, and autostop duration distinct. |
| !7207 | M | Stable-branch counterpart of !7208. |
| !7206 | M | Qt6Multimedia RTP-player compatibility work; platform CI failures were investigated in the actual build environment. |
| !7205 | L | Automatic generated-data/documentation update. |
| !7204 | L | Automatic stable-branch generated-data/documentation update. |
| !7203 | L | Automatic generated-data/registry update. |
| !7202 | M | Traffic column visibility is proxy/view state and is profile-specific. |
| !7201 | H | ftypes moves from generic pointer retrieval to typed accessors. |
| !7200 | L | TrafficTree label/prototype correction. |
| !7199 | L | BSSGP identifier spelling cleanup. |
| !7198 | M | MEGACO alternate control-flow path restores parser bracket-state invariants. |
| !7197 | L | Default UI layout change. |
| !7196 | M | Packet-analysis and log-analysis namespaces get appropriate default-profile behavior. |
| !7195 | L | Example plugin install-path correction. |
| !7194 | M | Traffic-type ordering made deterministic and sorting enabled explicitly. |
| !7193 | M | Alexis La Goutte enforces Wireshark commit-message format; corroborates existing notebook guidance. |
| !7192 | L | Release-note documentation. |
| !7191 | M | openSAFETY conversation support; later !7228/!7235 are stronger AT_NUMERIC precedent. |
| !7190 | M | Traffic-type UI uses a sort/filter proxy rather than source-model sorting policy. |
| !7189 | M | AT_NUMERIC introduction; later !7228/!7235 supersede its representation details. |
| !7188 | M | Idle dissection schedules at the next event-loop opportunity instead of an arbitrary delay. |
| !7187 | H | Conversation endpoint-by-ID must consistently use the `conv_elements` identity representation. |
| !7186 | M | Master MEGACO parser-state fix corresponding to !7198. |
| !7185 | M | Application namespace/version selection centralized for packet-vs-log tools. |
| !7184 | M | EtherCAT field masks corrected for field width and little-endian semantics. |
| !7183 | H | Heuristic recognition checks captured length before reading discriminator bytes. |
| !7182 | H | Stable counterpart of the RTCP heuristic length guard. |
| !7181 | L | Add direct includes instead of relying on transitive headers. |
| !7180 | H | John Thacker distinguishes Decode As default/reset, explicit Data, and no explicit binding. |
| !7179 | H | Master EtherCAT mask correction. |
| !7178 | L | Protocol naming/release-note update. |
| !7177 | M | Stable CI documentation glob correction to recursive `**/*`. |
| !7176 | M | Stable counterpart of CI glob correction. |
| !7175 | L | Stable release version bump. |
| !7174 | M | Master CI recursive glob correction. |
| !7173 | H | Master RTCP heuristic captured-length guard. |
| !7172 | L | Release build bookkeeping. |
| !7171 | L | AUTHORS wording cleanup. |
| !7170 | L | External-project acknowledgements maintenance. |
| !7169 | M | Author parser stops at the structural Acknowledgements boundary. |
| !7168 | M | Decode As offers packet-derived selector values even when only one is present. |
| !7167 | H | CLI option migration updates implementation, docs, and tests; John Thacker caught stale tests. |
| !7166 | L | IEEE 802.11 reason-code refresh. |
| !7165 | M | User guide updated with significant Traffic Tabs behavior. |
| !7164 | M | Localized Qt6 size-type conversions resolve warning-prone API boundaries. |
| !7163 | H | John Thacker: wildcard UDP conversation lookup must account for direction, ownership, tuple reuse, and recency. |
| !7162 | L | TFTP spelling-only backport. |
| !7161 | L | WSUG punctuation correction. |
