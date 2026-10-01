# Review findings — Wireshark MRs !2211–!2260

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Merged MRs are weighted more heavily than abandoned/superseded work. Direct maintainer guidance is called out where it materially strengthens a convention.

| MR | Outcome | Depth / authority | Finding |
|---:|---|---|---|
| !2260 | merged | Scanned / Gerald Combs | Keeps EditorConfig aligned with actual Wireshark style rather than upstream CMake's unrelated indentation convention; removes an obsolete per-file exception. |
| !2259 | merged | Deep / João Valverde | Introduces a common ws_debug() path and replaces scattered debug-print mechanisms; debug behavior should be centralized rather than encoded through ad hoc printf variants. |
| !2258 | merged | Scanned / João Valverde | Corrects CheckAPI target classification: the stats-tree plugin is not a dissector plugin, so it should not inherit dissector-only policy. |
| !2257 | merged | Deep | Stable-branch form of the GQUIC unknown-tag fix: preserve parser progress and valid extension handling while retaining the overflow guard that prevents the earlier infinite loop. |
| !2256 | merged | Scanned | Moves ASAP/ENRP shared definitions into a common module, reducing duplicated protocol constants and value strings. |
| !2255 | merged | Deep / Martin Mathieson + John Thacker | check_typed_item_calls.py exposed a 2-byte field registered as FT_UINT8; John checked the specification and confirmed the field is 16-bit, so the registration was corrected to FT_UINT16. |
| !2254 | merged | Deep / Gerald Combs | Keeps the TVBuff subset-search regression tests even though !2556 supplied the implementation fix differently. Tests for the contract remain useful independently of the chosen implementation. |
| !2253 | merged | Deep / Anders Broman | Anders asks for the standard proto_tree_add_bitmask() API for packed receipt flags; the accepted revision adopts it. |
| !2252 | merged | Scanned / Gerald Combs | Normalizes UI source indentation and modelines to the directory's established style. |
| !2251 | merged | Scanned / João Valverde | Corrects CheckAPI inputs for the stats-tree plugin instead of treating generated/plugin boilerplate as ordinary checked source. |
| !2250 | closed | Discussion / João Valverde | The proposed dissector unit-test suite had a promising model—generated TVBs plus direct proto-tree assertions—but João objected to changing production linkage merely to expose internals for tests. Useful design evidence, not accepted architecture. |
| !2249 | merged | Scanned | Release-3.4 backport of the accepted GQUIC unknown-valid-tag parser fix. |
| !2248 | merged | Deep | Fixes GQUIC so valid but unsupported tags advance by their declared length rather than aborting the list; separately retains an overflow/progress check to prevent malformed-input infinite loops. |
| !2247 | merged | Scanned | Restores the proper standard header for the snprintf declaration after an obsolete wrapper header was removed. |
| !2246 | merged | Deep / Guy Harris | Guy fixes a format-width mismatch and replaces temporary-buffer formatting plus col_append_lstr() with direct col_append_fstr(). |
| !2245 | merged | Deep / Guy Harris + João Valverde | Removes obsolete ws_snprintf wrappers. Guy asks whether any g_snprintf uses still need to remain; Wireshark's supported baseline now permits the C99 snprintf family directly. |
| !2244 | merged | Deep / Pascal Quantin | Pascal corrects PDCP-NR assumptions about out-of-order delivery and HFN state, rejects an unnecessary preference for behavior the protocol already permits, and warns that robust COUNT/HFN tracking must follow TS 38.323 under retransmission/reordering. |
| !2243 | merged | Deep / John Thacker | When table ID 0x3E has multiple legitimate interpretations, registers the common MPE interpretation as the single default and exposes Decode As for DSM-CC or private alternatives instead of relying on registration order. |
| !2242 | merged | Scanned | Automated release-3.2 data/translation refresh. |
| !2241 | merged | Scanned | Automated release-3.4 data/translation refresh. |
| !2240 | merged | Scanned | Automated master data/translation/documentation refresh. |
| !2239 | merged | Deep / Alexis La Goutte | New Opus dissector review explicitly requests both a representative capture and a release-note entry; the contributor supplies the pcap and documents the dynamic RTP payload configuration needed to reproduce it. |
| !2238 | merged | Deep / Guy Harris | Returns the named WTAP_FILE_TYPE_SUBTYPE_UNKNOWN sentinel for an impossible file-type lookup and checks it at callers so the internal failure is surfaced instead of passing an undecorated -1. |
| !2237 | merged | Deep / John Thacker | Uses semantic FT_ETHER only when the address is actually intelligible; scrambled bytes stay FT_BYTES, the payload is not falsely decoded, and source ranges are preserved for byte highlighting. |
| !2236 | merged | Discussion / Alexis La Goutte | Adds the 802.11 FTM Synchronization Information tag with review of the new field handling. |
| !2235 | merged | Deep / Guy Harris | Guy explains why stdout/stderr are not a reliable GUI diagnostic channel across macOS, GNOME, KDE, and other launch environments. Wireshark needs a central logging/error policy that makes diagnostics accessible rather than merely banning output calls. |
| !2234 | merged | Scanned | Expands the example plugin documentation to make the intended structure clearer. |
| !2233 | merged | Scanned / João Valverde | Skips the plugin-count regression when plugin support is disabled, keeping tests conditional on the feature configuration they exercise. |
| !2232 | merged | Scanned | First stage of the ASAP/ENRP common-code refactor, moving duplicated definitions and value strings to shared ownership. |
| !2231 | merged | Scanned | Adds ZVT Print Text Block dissection. |
| !2230 | merged | Scanned | Cleans DVB-CI protocol-column composition when invoking the MIME subdissector, avoiding confusing nested protocol text. |
| !2229 | closed | Deep discussion / Martin Mathieson, João Valverde, Gerald Combs | The proposal to disable commit-message length enforcement was rejected. Review converged on keeping useful policy but catching it locally with a commit hook where possible, with CI as a backstop rather than surprising contributors late. |
| !2228 | merged | Discussion / Alexis La Goutte | TLS-SRP support adds username dissection and decryption cipher suites; review requests a pcap and the contributor supplies one. |
| !2227 | merged | Deep | DNS packets quoted inside ICMP/ICMPv6 errors no longer mutate normal request/response correlation or retransmission state. Error-packet dissection is observational context, not a live transaction event. |
| !2226 | merged | Deep / Anders Broman | Adds RFC 4985 ASN.1 source/configuration and the matching regenerated PKIX Qualified dissector output together. |
| !2225 | closed | Low weight | Draft protest change combining commit-policy and unrelated LLDP edits; no accepted implementation or substantive review. |
| !2224 | merged | Deep / Guy Harris | Makes WTAP_FILE_TYPE_SUBTYPE_UNKNOWN an out-of-band -1 sentinel rather than a valid table index and removes the fake “unknown” registry entry. |
| !2223 | merged | Deep / Alexis La Goutte + Anders Broman | Corrects RSNX offset/length handling after the earlier patch was insufficiently tested; review pushes the implementation toward the existing capability-processing pattern, and the author validates both 1- and 2-octet captures. |
| !2222 | merged | Discussion | Extends GQUIC tag decoding and supplies focused example captures. |
| !2221 | merged | Scanned | Release-3.4 backport for GQUIC CGST decoding. |
| !2220 | closed | Low weight | Superseded/abandoned release-3.4 form of the CGST change; accepted behavior is represented by merged work. |
| !2219 | merged | Scanned / Alexis La Goutte | Corrects FILS Discovery parsing and removes a dead store. |
| !2218 | merged | Deep / Guy Harris | Adds a common Wiretap block type for systemd journal entries because the semantic record type can be used by more than one file format. |
| !2217 | merged | Deep / Guy Harris | Validates file-type/subtype indices before table access and restructures dumper initialization so validation occurs once before the validated value is stored and reused by later helpers. |
| !2216 | merged | Scanned | Extends RSNX parsing using a modular per-octet structure intended to scale to future capability octets. |
| !2215 | merged | Deep / Guy Harris | Renames the registration API to singular because it registers one subtype, validates that registrations support meaningful block types, and deliberately makes stale plugins rebuild so structural incompatibilities surface at compile/registration time. |
| !2214 | merged | Deep / Guy Harris | Fixes a function definition whose argument order disagreed with its declaration and all call sites; Coverity detected the latent contract mismatch. |
| !2213 | merged | Scanned | Reports an unrecognized Git pkt-line special value through expert info instead of silently treating it as ordinary data. |
| !2212 | merged | Scanned / Guy Harris | Updates the usbdump Wiretap plugin to the new file-type/subtype structure, including supported packet/block information. |
| !2211 | merged | Scanned | Fixes Git pkt-line parsing to honor the caller-supplied offset when multiple records share one TVBuff. |

No SMPTE ST 291/VANC packet type was encountered in this batch.
