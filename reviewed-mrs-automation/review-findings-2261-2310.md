# Review findings — Wireshark MRs !2261–!2310

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

| MR | Outcome | Depth / authority | Finding |
|---:|---|---|---|
| !2310 | merged | Deep / Guy Harris | Per-packet 802.11 modulation can differ from channel metadata; rank direct packet evidence above ambiguous channel flags. |
| !2309 | merged | Discussion | JSON-RPC work; cross-platform review caught narrowing, constness, assignment-condition, and commit-message issues. |
| !2308 | merged | Scanned | Release 3.4 preparation. |
| !2307 | merged | Scanned | Release 3.2 preparation. |
| !2306 | merged | Scanned | LTE-RRC field-name cleanup keeps ASN.1 config and generated output synchronized. |
| !2305 | merged | Scanned | Removes decoding for data not present in the GBCS alert. |
| !2304 | merged | Deep / Pascal Quantin | Dispatches embedded NAS to EPS or 5GS using the embedded discriminator; updates generator inputs and output. |
| !2303 | merged | Scanned | Moves repeated environment lookup to initialized process state. |
| !2302 | closed | Scanned | Unmerged DPoE change; no substantive review. |
| !2301 | merged | Deep / John Thacker | Stops PSI reassembly at the declared section boundary and treats the remaining TS payload as stuffing. |
| !2300 | merged | Discussion | Hex-copy display change; Anders Broman suggests clearer reusable conversion handling. |
| !2299 | merged | Scanned | Automatic data update. |
| !2298 | merged | Scanned | Automatic data update. |
| !2297 | merged | Scanned | Automatic data and translation update. |
| !2296 | closed | Discussion | João Valverde rejects a no-op dissector used only to set columns; semantic behavior belongs with the decoder that understands the data. |
| !2295 | merged | Deep | Adds registration checks for expert-info group and severity after finding swapped or invalid values. |
| !2294 | merged | Discussion | NetPerfMeter naming cleanup. |
| !2293 | merged | Scanned | RPM Unix Makefiles build fix. |
| !2292 | merged | Scanned / Guy Harris | Network Instruments capture-format maintenance. |
| !2291 | merged | Deep / Guy review | Fixes ptvcursor copy-size bug; Guy recommends wmem_realloc(), later adopted in !2724. |
| !2290 | merged | Scanned | 802.11az LMR support. |
| !2289 | merged | Scanned | Corrects value_string numeric key. |
| !2288 | merged | Deep | Marks response value derived from request state as generated and checks its semantic value domain. |
| !2287 | closed | Scanned | Duplicate of !2288. |
| !2286 | merged | Scanned | Corrects MBIM value_string numeric key. |
| !2285 | merged | Scanned | RPM setup option typo. |
| !2284 | merged | Deep | Uses test assertions that remain active independently of normal runtime assertion policy. |
| !2283 | merged | Discussion | Bluetooth review identifies calculated access addresses as generated values. |
| !2282 | merged | Scanned | ZVT refund and reversal support. |
| !2281 | merged | Deep | Separates concise capture-capability error text from longer remediation guidance. |
| !2280 | merged | Deep | Preserves explicit GLib log-domain configuration and uses domain-aware handling. |
| !2279 | merged | Scanned | Radiotap typo fix. |
| !2278 | merged | Scanned / Guy Harris | Snoop documentation clarification. |
| !2277 | merged | Scanned | Qt modeline cleanup. |
| !2276 | merged | Deep / Guy Harris | Infers legacy 802.11 PHY from rate and band evidence. |
| !2275 | merged | Discussion / Guy Harris | Renames helper module to match its broadened 802.11 responsibility. |
| !2274 | merged | Deep | Corrects Lua FieldInfo ordering to documented half-open interval behavior. |
| !2273 | merged | Deep | Keeps expensive SCTP association indexing disabled by default; avoids expert-info flooding for the disabled state; review also enforces commit cleanup. |
| !2272 | merged | Scanned | Small NetPerfMeter result fix. |
| !2271 | merged | Scanned | Stops shipping build-private config.h as a public development header. |
| !2270 | merged | Discussion | Anders Broman discourages repeated rebases that only consume CI while review comments remain. |
| !2269 | merged | Scanned | ZVT status information fields. |
| !2268 | merged | Deep | Defines runtime assertion policy; assertions are for internal invariants and are not substitutes for packet or safety checks. |
| !2267 | merged | Scanned / Guy Harris | NetXray comment update. |
| !2266 | merged | Discussion | 802.11 constant naming and value-range completeness review. |
| !2265 | merged | Scanned | Fixes warnings found with optimized GCC build. |
| !2264 | merged | Deep | João Valverde favors small captures and stable semantic assertions over large snapshots of ordinary tshark text. |
| !2263 | merged | Scanned | Adds ENRP statistics. |
| !2262 | merged | Scanned | O-RAN parameter and reference cleanup. |
| !2261 | merged | Discussion | Anders Broman reinforces protocol-prefixed hf variable naming. |
