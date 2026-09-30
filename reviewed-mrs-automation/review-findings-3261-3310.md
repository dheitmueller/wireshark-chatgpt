# Review findings: 3261-3310

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

| MR | Outcome | Review note |
|---|---|---|
| 3310 | merged | wslog domain filtering; Critical/Error remain unconditional. |
| 3309 | merged | Commit validator strips template comment lines before format checks. |
| 3308 | merged | Kerberos optional-build fix made in ASN.1 conformance source and regenerated C. |
| 3307 | merged | Guy Harris removes source-file BOMs. |
| 3306 | merged | 3.2 backport of argv NULL-termination fix. |
| 3305 | merged | 3.4 backport of argv NULL-termination fix. |
| 3304 | merged | Gerald Combs fixes GLib internal-header discovery per selected/platform library layout. |
| 3303 | merged | UTF-8 argv conversion allocates argc+1 and writes terminating NULL. |
| 3302 | merged | Osmocom TRX exposes shadow classification as registered boolean while retaining value. |
| 3301 | merged | RDP split accepted for coherent growth/reuse; review also catches wrong field width. |
| 3300 | merged | PFCP enterprise-ID and expert-field cleanup. |
| 3299 | merged | NVMe/RDMA CM field and presentation corrections. |
| 3298 | closed | One-platform GLib hint rollback rejected; superseded by 3304. |
| 3297 | merged | Project-wide migration toward shared wslog API. |
| 3296 | merged | 3.2 Wi-Fi NAN length backport. |
| 3295 | merged | 3.4 Wi-Fi NAN length backport. |
| 3294 | merged | DVB-S2-BB composite-TVBuff off-by-one fix. |
| 3293 | merged | Disabled debug/assert macros keep argument references to avoid unused warnings. |
| 3292 | merged | Windows GLib dependency/package-layout update. |
| 3291 | merged | Wi-Fi NAN availability minimum length corrected. |
| 3290 | merged | Signed geographical fields use signed extraction API/type. |
| 3289 | closed | Guy Harris defines payload-vs-keyed dissector tables and overlapping CAN namespaces. |
| 3288 | merged | Qt printer-dialog object selection fix. |
| 3287 | merged | DVB-S2-BB CRC offset widened to natural offset type. |
| 3286 | merged | User Guide Debian README link cleanup. |
| 3285 | merged | WoW later-version support. |
| 3284 | merged | pcapng locals crossing longjmp boundary made volatile. |
| 3283 | merged | PTP dissectors registered by name for scripting lookup. |
| 3282 | merged | Removes unused static-checker option. |
| 3281 | merged | SMB adapts to export-object size_t payload length. |
| 3280 | merged | Leak fix; Gerald Combs requires valid author metadata and commit format. |
| 3279 | merged | Kerberos leak fix updates template and generated C; same author-metadata requirement. |
| 3278 | merged | 802.11 aggregate-duration description clarified. |
| 3277 | merged | DVB-S2-BB direction state made explicit before stream processing. |
| 3276 | merged | CI job avoids overriding inherited before_script. |
| 3275 | merged | Raw display-filter strings documented. |
| 3274 | merged | Adds code-line counters; CI composition corrected by 3276. |
| 3273 | merged | 802.11 presentation rename. |
| 3272 | merged | Qt centralizes last-open-directory derivation from filenames. |
| 3271 | merged | Export Object payload_len becomes size_t, following Guy Harris type-domain guidance. |
| 3270 | merged | GTPv2 add-item-and-return avoids a second field fetch. |
| 3269 | closed | Live-capture Lua reload draft; no substantive review. |
| 3268 | closed | Guy rejects gint64 for resident-buffer length; points to size_t model implemented by 3271. |
| 3267 | merged | TLS session exporter returns authoritative byte length with buffer. |
| 3266 | merged | Regex/display-filter escaping documentation corrected. |
| 3265 | merged | Display-filter macro documentation reference fixed. |
| 3264 | merged | Guy Harris flags unrelated qcustomplot edits twice. |
| 3263 | merged | Wiretap migrates assertions to Wireshark assertion wrapper. |
| 3262 | merged | ws_debug supplies function provenance centrally; callers remove manual prefixes. |
| 3261 | merged | Spelling list and field-name typo cleanup. |

Merged work is primary precedent. Closed 3298, 3289, 3269 and 3268 are down-weighted; merged successors are preferred where available. Guy Harris feedback in 3289, 3268 and 3264 is retained as high-authority review evidence.
