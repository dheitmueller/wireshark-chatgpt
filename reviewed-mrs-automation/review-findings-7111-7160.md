# Wireshark MR review findings: !7111–!7160

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed MRs were reviewed, descending from !7160 through !7111. Merged work is weighted above closed/unmerged work.

| MR | Outcome | Finding |
|---|---|---|
| !7160 | merged | Qt traffic sorting: fix role tests and sort ports numerically. |
| !7159 | merged | MaxMind map/model-role fix; part of the role logic was immediately refined by !7160. |
| !7158 | merged | Reassembled GSM 7-bit text keeps its actual unpacked GSM encoding rather than ASCII. |
| !7157 | merged | Traffic sorting moves toward typed/unformatted values and semantic secondary keys. |
| !7156 | merged | **Deep.** John Thacker deliberately avoids always filling a wildcarded TFTP server port because exact-tuple lookup plus port reuse can select an older conversation. |
| !7155 | merged | MCPTT Track Info padding parsing and diagnostics. |
| !7154 | merged | MCPTT fix distinguishes a referenced text-encoding rule from enclosing SDES wire fields. |
| !7153 | merged | SMPP packed GSM 7-bit + UDH computes fill bits/septet count before decode. |
| !7152 | merged | Automatic data/translation update; no durable lesson. |
| !7151 | merged | **Deep.** `ip.flags` gets mask `0xE0`, matching its three-bit semantics; release notes and baselines acknowledge display-filter compatibility impact. |
| !7150 | merged | Automatic translation update. |
| !7149 | merged | Automatic update; no durable lesson. |
| !7148 | merged | Expert dialog initializes display-filter limiting from actual active state. |
| !7147 | merged | TFTP spelling cleanup. |
| !7146 | merged | Traffic sorting refactor toward typed values; later refined by !7157/!7160. |
| !7145 | merged | SMPP passes TLV/UDH-derived metadata through shared state for GSM SMS UD subdissection. |
| !7144 | merged | TShark `read_format` documentation. |
| !7143 | merged | GSM SMS header include guard. |
| !7142 | merged | Reuses the more complete GSM SMS UDH parser instead of maintaining a weaker duplicate. |
| !7141 | merged | **Deep.** Guy Harris and John Thacker: sanitized `ENC_ASCII` text may contain UTF-8 replacement characters, so converted byte length is not a wire offset. Prevalidate required ASCII and stop structural parsing on invalid wire text. |
| !7140 | merged | Pascal Quantin requested naming-style, typo, unit, and draft-state cleanup. |
| !7139 | merged | AT CMGL dissection; contributor supplied an example pcapng. |
| !7138 | merged | Release notes document the tap callback signature change. |
| !7137 | merged | Release-note cleanup. |
| !7136 | merged | Traffic-dialog release notes. |
| !7135 | merged | Thrift UUID exported helper also updates Debian symbol metadata; contributor reports fuzz/check coverage. |
| !7134 | merged | **Deep.** John Thacker removes context-poor `fragment_get_reassembled()`; lookup moves to `fragment_get_reassembled_id(table,pinfo,id)` so completed-reassembly identity can include packet context. |
| !7133 | merged | Mutually exclusive GSM character-set state becomes one enum instead of several booleans. |
| !7132 | merged | **Deep.** Tap filtering changes from dropping nonmatching packets to carrying match state so consumers can compute total and filtered statistics from one event stream. |
| !7131 | merged | **Deep.** README.dissector clarifies that string encodings without byte-order semantics use the encoding alone; do not mechanically OR `ENC_NA`. |
| !7130 | merged | Encoding-argument cleanup script follow-up. |
| !7129 | merged | PortableApps version-variable cleanup. |
| !7128 | merged | **Deep.** Tap callback flags carry filter/match metadata instead of requiring duplicate taps or UI-side re-filtering. |
| !7127 | merged | SMPP message-payload TLV is decoded using DCS-derived encoding. |
| !7126 | closed | Closed broad encoding cleanup; down-weighted. !7131 is the stronger merged precedent. |
| !7125 | merged | GSM SMS language-shift IEIs. |
| !7124 | closed | **Review-only.** Guy Harris argues UTF-16 byte order should remain composable with protocol byte-order flags; João Valverde abandoned the change. |
| !7123 | merged | Adds the standard header needed for `bsearch()` portability. |
| !7122 | merged | Gerald Combs requests pointer initialization to NULL and guarding before `strcmp()`; accepted. |
| !7121 | merged | SMPP DCS decoding corrected to SMPP's actual allowed semantics. |
| !7120 | merged | WiX/PortableApps target rename updates CI and packaging docs together. |
| !7119 | merged | RPM/AppImage target rename updates CI, INSTALL, and developer docs together. |
| !7118 | merged | Adds/generalizes macOS Logwolf DMG packaging. |
| !7117 | merged | Traffic-dialog UI redesign; no substantive review captured. |
| !7116 | merged | Actual diff removes duplicate Qt signals despite stale MR title; reviewed the diff, not the title. |
| !7115 | merged | **Deep.** John Thacker catches that removing apparently GUI-only command paths also affects registered `-z` CLI commands, help/manpage exposure, I/O Graph launch, and shared tshark registration. |
| !7114 | merged | Docs update regex provider from GRegex to PCRE2. |
| !7113 | merged | **Deep.** Case-insensitive display-filter regex semantics land with docs, tests, and a lower-level compile-flags API. |
| !7112 | merged | IrDA conversation creation updated for changed conversation option semantics. |
| !7111 | merged | Display-filter checkbox reflects whether a filter actually exists. |

## Durable conclusions

- Converted/sanitized packet text is not wire-coordinate storage; validate the wire encoding before structural parsing and never map converted-string byte lengths back to TVB offsets (!7141).
- Completed-reassembly lookup must preserve the same semantic identity dimensions needed to distinguish reassemblies; frame/id alone may be insufficient (!7134).
- Wildcard UDP conversation specificity must account for generic lookup precedence and tuple reuse; “more specific” can be wrong (!7156, corroborating !7163).
- Header-field masks define user-visible field semantics; correcting a mask can be a display-filter compatibility change and should bring tests/release notes (!7151).
- Tap consumers may need match metadata rather than pre-filtered event loss; carrying flags through the tap event enables total+filtered statistics without duplicate traversal (!7128/!7132, documented by !7138).
- Character encoding and byte order are separate dimensions. Use the string encoding alone when byte order is inapplicable; keep endian selection composable where protocol context supplies it (!7131; !7124 is supporting closed review evidence).
- Shared frontend registration is an external contract. Before deleting a GUI-looking callback or parameter, trace CLI registrations, help/manpages, and tshark consumers (!7115).
