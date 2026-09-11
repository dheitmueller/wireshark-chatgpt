# Wireshark MR review batch !24109–!24158

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

## Selection and prior-review accounting

Before selecting this batch, the already-reviewed set was rebuilt from `reviewed-mrs.md`, the aggregate `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers under `reviewed-mrs-automation/`. MR numbers were counted individually rather than inferring coverage from ledger filenames. This matters because the aggregate ledger contains the separately reviewed !25759–!25827 material that is not represented by a matching per-run filename sequence. The historical !17571–!17620 batch remains preserved and counted.

After subtracting every individually tracked reviewed MR from the corpus and sorting descending, the 50 highest-numbered unreviewed MRs are exactly **!24158 through !24109**, inclusive.

Count: **50**.

## Exact reviewed set and findings

| MR | Review | Notes |
|---|---|---|
| !24158 | Scanned | Qt RTP Streams initializes tree items from the current Time-of-Day setting. Merged UI-state fix; no new core convention. |
| !24157 | Deep | HTTP Upgrade table now uses case-insensitive string matching because the protocol-name token is normatively case-insensitive. Promoted registration-semantics rule. |
| !24156 | Scanned | Follow Stream keeps the configured font while zooming. UI behavior fix. |
| !24155 | Scanned | TRDP frees values returned by libxml2 `xmlGetProp()`. Straightforward third-party ownership fix. |
| !24154 | Scanned | Packet-list column state is no longer restored at an inappropriate file-open lifecycle point. UI state fix. |
| !24153 | Deep | dumpcap grows the pipe buffer for large pcapng SHBs, validates minimum size, and postpones endian-sensitive checks until byte order is known. Guy Harris also favored making the roundup helper generally usable; strong corroboration of existing capacity/input-order guidance. |
| !24152 | Discussion-focused (closed) | Proposed Welcome Screen backport. Closed/unmerged, therefore down-weighted. |
| !24151 | Scanned | Stable backport of SANE illegal-option progress fix. Corroborates !24145. |
| !24150 | Scanned | Stable backport of SANE illegal-option progress fix. Corroborates !24145. |
| !24149 | Scanned | Welcome slide rotation starts after splash overlay closes. Merged UI timing fix. |
| !24148 | Scanned (closed/superseded) | Earlier submission of the slide-rotation change; down-weighted in favor of merged !24149. |
| !24147 | Scanned | Bluetooth LMP feature page/field/filter corrections. Protocol-specific correctness work. |
| !24146 | Scanned | RTPS built-in InstanceStateDataResponse is dissected independently of the user-data preference and rendered with standard GUID/time/sequence helpers. Mostly protocol-specific consistency. |
| !24145 | Deep | SANE handles illegal/size-less option types without zero-progress looping by advancing according to the reported size. Strong monotonic-progress corroboration. |
| !24144 | Scanned | MACsec displays Ethertype as part of decrypted payload at its semantic location. Presentation correction. |
| !24143 | Deep | ASN.1 REAL malformed/range handling replaces packet-triggerable assertions with explicit status plus expert diagnostics. Guy Harris requested an explicit out-parameter for errno-style status; John Thacker incorporated it. Promoted parser-error-contract rule. |
| !24142 | Scanned | Qt splash/extcap progress path refactor. Merged UI/startup work; no durable core rule promoted. |
| !24141 | Scanned | Qt info-widget click/highlighting cleanup. UI-only. |
| !24140 | Discussion-focused | extcap/libssh chooses Windows config semantics by runtime libssh version and sets logging before config parsing because that API can override options. Useful third-party API-ordering example, but too integration-specific for a standalone rule. |
| !24139 | Scanned | Documentation for CMakeUserPresets.json. Documentation-only. |
| !24138 | Scanned | Seasonal welcome-banner support. UI/content feature. |
| !24137 | Scanned | O-RAN FH CUS cleanup. No distinct reusable convention. |
| !24136 | Scanned | Adds explanatory comment for extcap SSH workaround. Documentation/maintenance. |
| !24135 | Scanned | RTPS domain-ID calculation includes domain-zero multicast boundary. Protocol-specific off-by-one fix. |
| !24134 | Scanned | Stable Kismet heap-overflow hardening using tree/TVB APIs and ASCII validation. Corroborates master !24124 and existing text/bounds guidance. |
| !24133 | Scanned | Sibling stable Kismet hardening backport; no additional lesson. |
| !24132 | Deep | LZ77 decompression rejects NULL/zero/oversized input before allocating. Strong pre-allocation validation corroboration. |
| !24131 | Scanned | Stable backport of LZ77 decompression-size cap preventing pathological long loops. |
| !24130 | Scanned | Sibling stable backport of LZ77 size cap. |
| !24129 | Scanned | CIP vendor registry additions. Data maintenance. |
| !24128 | Deep | `wmem_array` growth arithmetic is checked for overflow and reports critical failure without assuming every caller must abort. Strong checked-capacity corroboration. |
| !24127 | Scanned | CIP enables fully supported StringN dissection and removes obsolete expert warning. Protocol-specific completion. |
| !24126 | Scanned | Stable zlib/format-text overflow hardening. Corroborates existing decompression/allocation arithmetic guidance. |
| !24125 | Deep | LZ77 decompression applies an explicit implementation limit to an attacker-controlled 32-bit expansion size that otherwise permits pathological execution. Corroborates bounded decompression/resource limits. |
| !24124 | Deep | Kismet master fix replaces unsafe string extraction with protocol-tree/TVB APIs and corrects ASCII/single-character handling. Corroborates existing encoding-aware text and TVB guidance. |
| !24123 | Scanned | Backports checked-integer support to 4.4 to make later safety fixes easier to backport. Infrastructure maintenance. |
| !24122 | Scanned | Stable extcap/libssh logging-level compatibility fix. Integration-specific. |
| !24121 | Scanned | Sibling extcap/libssh logging backport; no additional lesson. |
| !24120 | Scanned | Stable USB HID fix skips zero-size report items that otherwise permit huge non-progress loops. Corroborates master !24114. |
| !24119 | Scanned | Sibling USB HID zero-size-item backport. |
| !24118 | Scanned | CIP File Object coverage expansion. Protocol feature work without distinct review lesson. |
| !24117 | Deep | DLMS/COSEM prevents zero-progress compact-array loops, advances unknown types, and uses a subset TVB to enforce the declared contents boundary. Strong corroboration of monotonic-progress and subset-TVBuff rules. |
| !24116 | Discussion-focused | Large merged DIS stream-analysis feature includes sample-capture regression coverage and a dedicated DIS fuzz target. Useful testing corroboration; no new standalone testing rule. |
| !24115 | Scanned | Master extcap/libssh logging compatibility fix. No general rule beyond respecting third-party version behavior. |
| !24114 | Deep | USB HID skips report-size-zero items because they consume no bytes and can otherwise produce attacker-controlled long loops. Strong monotonic-progress/resource-bound corroboration. |
| !24113 | Scanned | RTPS introduces dedicated filter fields where one generic sequence-number field prevented precise filtering; protocol-specific field semantics improvement. |
| !24112 | Scanned | RTPS builtin-type and secure TypeLookup entity-ID corrections. Protocol-specific correctness. |
| !24111 | Scanned | RTPS moves standard PID_TYPE_OBJECT handling out of an RTI-vendor-only path and aligns offsets for standard padding semantics. Good standards-vs-vendor correction, but no new broad rule beyond existing generic-vs-vendor dispatch guidance. |
| !24110 | Scanned | Dissector warning cleanup. No distinct durable lesson. |
| !24109 | Deep | Coloring Rules fixes ownership/clang issues and changes GUI effect flags so a coloring-only change triggers recolor rather than unnecessary full redissect. Strong corroboration of later preference/effect-scoping guidance. |

## Durable findings promoted

- `protocol-input-diagnostics-conventions.md`: shared conversion helpers should explicitly return malformed/range status alongside decoded values when callers need to classify diagnostics; packet input must not become an assertion (!24143). Guy Harris's concrete API suggestion was incorporated before merge.
- `dissector-table-registration-conventions.md`: string dissector-table comparison semantics should match the protocol's normative case-sensitivity, rather than depending on every caller/registration to normalize manually (!24157).

Strong !24153, !24145, !24132, !24128, !24125, !24124, !24117, !24114, and !24109 primarily corroborated notebook guidance already present for capacity validation, monotonic progress, bounded decompression, safe TVB/text APIs, subset TVBs, and state/effect scoping, so those rules were not duplicated.