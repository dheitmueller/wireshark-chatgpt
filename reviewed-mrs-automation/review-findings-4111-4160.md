# Review findings: !4111–!4160

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This file records the per-MR result of the exact 50-MR review batch. Merged MRs are weighted more heavily than closed or superseded submissions. Maintainer-authored and maintainer-reviewed evidence—especially Guy Harris guidance—is weighted according to authority and relevance.

| MR | Outcome | Depth | Finding |
|---:|---|---|---|
| !4160 | merged | Scanned | release-3.4 backport of !4159; no additional architectural lesson. |
| !4159 | merged | Scanned | Reads the newer Windows DisplayVersion registry value with ReleaseId fallback; author reported testing on Windows 10 21H1. |
| !4158 | merged | Scanned | Adds a hex-digit decode mode that skips non-hex characters and updates the User Guide. |
| !4157 | merged | Scanned | Guy Harris makes a shebang-based developer script executable so it can be invoked directly. |
| !4156 | merged | Deep | Guy Harris excludes the IDL dissector generator from checks written for compiled dissectors because checker pattern matching mistakes generator format strings for actual declarations. |
| !4155 | merged | Scanned | Guy Harris fixes spelling warnings in project-owned source while deliberately leaving externally sourced identifiers/spec data alone. |
| !4154 | merged | Deep | John Thacker adds real compression-type reporting; Guy Harris explicitly reviews file-local vs library-internal vs public API scope and the export/symbol-manifest obligations of the public case. |
| !4153 | closed | Discussion-focused | Gerald Combs marked the draft unsuitable to commit; discussion explored deriving MSVC redistributable locations from CMake/Visual Studio state rather than fragile transient environment setup. Unmerged, so design evidence only. |
| !4152 | merged | Scanned | Protocol-specific O-RAN extension support; author noted captures were not yet available at submission time. |
| !4151 | merged | Deep | Evan Huus initializes the next-TVB lists at the second H.225 dissector entry point and applies the fix in the ASN.1 template plus regenerated C. |
| !4150 | merged | Deep | John Thacker replaces a Boolean FCS preference with heuristic/never/always modes, but only consults it when Wiretap metadata does not already say whether the FCS is present. |
| !4149 | merged | Scanned | Guy Harris source organization cleanup; no new cross-cutting convention. |
| !4148 | merged | Scanned | Corrects RDP endianness and SOFT_SYNC_REQ channel-name display. |
| !4147 | closed | Discussion-focused | Unmerged profile-metadata design discussion; Stig Bjørlykke highlighted that copied profiles quickly make author/version provenance stale. Useful product-design history, not accepted implementation precedent. |
| !4146 | merged | Scanned | Guy Harris source organization cleanup; no new cross-cutting convention. |
| !4145 | merged | Scanned | Small UI status-text correction for JSON export. |
| !4144 | merged | Deep | Guy Harris adds a subtype-specific minimum-length check before reading the Netflix skip block's 32-bit value and reports malformed block lengths consistently as WTAP_ERR_BAD_FILE. |
| !4143 | merged | Scanned | master-3.2 backport of !4136; no additional lesson. |
| !4142 | merged | Scanned | Protocol-version compatibility update for BLIP. |
| !4141 | merged | Scanned | release-3.4 backport of !4136; no additional lesson. |
| !4140 | merged | Scanned | Guy Harris master-3.2 backport of the legacy CCMP AAD correction. |
| !4139 | merged | Scanned | Guy Harris release-3.4 backport of the legacy CCMP AAD correction. |
| !4138 | merged | Scanned | Master compatibility implementation for older libgcrypt; backported by !4139/!4140. |
| !4137 | merged | Deep | Evan Huus uses tree-owned allocation where the tree is the real owner and uses explicit temporary allocate/free where data need not survive, removing more ambient packet-scope dependence. |
| !4136 | merged | Deep | Credentials tap initialization can run for tshark live capture while epan scope is active but file scope is not; accepted fix allocates the retained array from epan scope. Stable backports followed. |
| !4135 | merged | Deep | Roland Knall makes adding/copying a UAT row one model insertion instead of many setData/dataChanged emissions, avoiding repeated downstream redissection. |
| !4134 | merged | Discussion-focused (superseded) | The semantic idea was correct, but this MR directly edited generated packet-sv.c. Guy Harris explicitly said to edit the ASN.1 template and regenerate; later Guy-authored !4202 is the authoritative implementation. |
| !4133 | merged | Scanned | Wiretap close now releases ZSTD and LZ4 decompression contexts in addition to the existing zlib cleanup. |
| !4132 | merged | Scanned | Protocol extension with attached sample captures; no new cross-cutting review rule. |
| !4131 | merged | Discussion-focused | Corrects BOOTP/DHCP naming and admits the man-page list is incomplete; broader documentation restructuring was deliberately left for separate work. |
| !4130 | merged | Scanned | Bundled dependency update includes release notes and packaging/setup changes, corroborating existing dependency-release-note practice. |
| !4129 | merged | Scanned | release-3.4 backport of the 802.11 AAD correctness fix. |
| !4128 | merged | Scanned | Dependency-version floor adjustment for Wiretap LZ4 support. |
| !4127 | merged | Deep | Evan Huus changes next_tvb lists to retain an explicit allocator and allocate all child items from that same pool; H.225/SNMP callers pass pinfo->pool and the exported symbol manifest changes with the API. |
| !4126 | merged | Discussion-focused | Jaap Keuter surfaced commit-check field-prefix/whitespace failures; Alexis La Goutte required cppcheck findings to be examined, while also identifying a false positive. The author explicitly noted that green status alone was insufficient without reading logs. |
| !4125 | merged | Scanned | Debug-only allocations no longer require ambient packet scope. |
| !4124 | merged | Deep | Evan Huus replaces many global packet-scope allocations with the protocol tree's owning pool when the tree is guaranteed non-NULL. |
| !4123 | merged | Deep | The code short-circuits on a NULL tree so remaining allocations can reliably use the tree's pool rather than ambient packet scope. |
| !4122 | closed | Discussion-focused | Roland Knall warned that replacing display-filter fields can break existing workflows and recommended preserving old fields while adding replacements and migration guidance. Closed without merge, so compatibility guidance only. |
| !4121 | merged | Scanned | Static-analysis cleanup in Qt, pcapng, and sharkd paths. |
| !4120 | merged | Scanned | Adds TLS Decode-As support for SOME/IP. |
| !4119 | merged | Scanned | Dictionary update; no new cross-cutting lesson. |
| !4118 | closed | Discussion-focused | Graham Bloice cautioned that removing filter fields can break user workflows and suggested adding the common field rather than replacing existing fields. Closed, so lower-weight compatibility guidance. |
| !4117 | merged | Scanned | Dictionary type update; no new cross-cutting lesson. |
| !4116 | merged | Deep | John Thacker classifies MPEG-TS continuity-counter loss as PI_SEQUENCE because missing progression does not make an otherwise valid packet malformed. This is the master-origin change later seen in stable !4474. |
| !4115 | merged | Deep | John Thacker calls stateful MP2T dissection on the first pass even without a display tree and keys per-frame MP2T analysis by curr_layer_num because one AVTP frame can contain several MP2T instances whose local TVB offsets repeat. A focused fragmentation capture accompanied the change. |
| !4114 | merged | Scanned | Automated release-3.2 data/translation refresh. |
| !4113 | merged | Discussion-focused | Adds differentiated protocol/application exception and unusual-field expert info; Anders Broman asked that an unrelated error-code/documentation issue be removed and handled separately. |
| !4112 | merged | Scanned | Automated release-3.4 data/translation refresh. |
| !4111 | merged | Scanned | Automated master data/translation refresh. |

## High-authority evidence in this batch

Guy Harris directly authored !4157, !4156, !4155, !4149, !4146, !4144, !4140, !4139, and reviewed !4154 and !4134. The strongest reusable evidence is !4156 (checker applicability follows source class), !4154 (visibility/export contract), !4144 (subtype-specific Wiretap length validation), and the !4134 → !4202 correction showing that generated dissector semantics belong in the authoritative ASN.1 template.

John Thacker authored merged !4154, !4150, !4116, and !4115. Evan Huus authored the allocator-scope series !4137, !4127, !4124, and !4123. These merged master changes carry substantially more weight than the four closed submissions in this batch.

Closed !4153, !4147, !4122, and !4118 are retained only as lower-weight design/review history. Merged !4134 is also treated cautiously because its direct edit to generated output was explicitly corrected by Guy Harris and superseded architecturally by Guy-authored !4202.
