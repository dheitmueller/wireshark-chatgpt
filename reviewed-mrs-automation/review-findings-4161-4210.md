# Wireshark MR review findings: 4161–4210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This file records the per-MR result of the exact 50-MR review batch. Merged MRs are weighted more heavily than closed/superseded submissions; maintainer-authored and maintainer-reviewed evidence is weighted according to authority and relevance.

| MR | Outcome | Depth | Finding |
|---:|---|---|---|
| !4210 | merged | Deep | João Valverde moved generic numerical/hex formatting helpers from epan to wsutil, updated library symbol manifests, and added wsutil tests. Strong lower-common-layer and ABI-placement corroboration. |
| !4209 | closed | Discussion-focused (superseded) | BPSec/BPv7 submission was superseded by later merged !4497. Review asked for release notes, wslog/expert info instead of stderr, check_static cleanup, reuse of wscbor rather than unsafe duplicate CBOR parsing, preservation of LGPL provenance, and clearer protocol-layer separation. |
| !4208 | merged | Deep | Jaap Keuter split Infiniband handoff work into one-time registration/handle creation and repeatable preference-selected UDP rebinding. Handoff callbacks must tolerate preference-driven re-entry. |
| !4207 | merged | Scanned | TWAMP control state no longer assumes only one Request-Session before Session-Start; message command number is used to distinguish legal transitions. |
| !4206 | merged | Deep | John Thacker makes TCP/UDP/SCTP try user-changed Decode As/preference table entries before default port-order heuristics, adding APIs that distinguish changed registrations from defaults. |
| !4205 | merged | Scanned | PFCP tree presentation improves Rule ID visibility; no new cross-cutting convention. |
| !4204 | merged | Scanned | macOS Homebrew setup script gains DMG-build dependencies/options; small build-setup change. |
| !4203 | merged | Scanned | Runtime dependency label changes from SMI to libsmi; naming cleanup without a broader new rule. |
| !4202 | merged | Deep | Guy Harris uses set_actual_length() for IEC 61850 SV so Ethernet can retain/dissect trailers/FCS and makes the semantic change in the ASN.1 template rather than generated packet-sv.c. |
| !4201 | merged | Scanned | NEWS HTML/text conversion gains nested-list handling; documentation tooling change. |
| !4200 | merged | Deep | Bit-order support is propagated through tvb/proto APIs, existing callers explicitly retain big-endian behavior, USB HID opts into little-endian bit numbering, and capture review exposed a hidden big-endian assumption in the FT_BYTES path that was then fixed. |
| !4199 | merged | Scanned | LZ4 compatibility for versions before 1.8.0; dependency-compatibility adjustment. |
| !4198 | merged | Scanned | wslog gains a validate-and-return macro similar to GLib precondition helpers. |
| !4197 | merged | Scanned | Adds wsutil test coverage for bytes_to_str_punct(). |
| !4196 | merged | Deep | Continues moving generic to_str_back helpers from epan into wsutil with tests and symbol-manifest migration; reinforces lower-layer ownership of generic formatting. |
| !4195 | merged | Deep | RDP explicitly associates TCP and UDP connections so dynamic-channel state can be shared across transport legs; useful cross-transport state-association example. |
| !4194 | merged | Deep | Evan Huus changes ptvcursor_new() to take an explicit allocator because the tree may be NULL or replaced; caller-visible scope is safer than inferring lifetime from presentation state. |
| !4193 | merged | Deep | Evan Huus makes tvbparse retain an explicit allocator and uses it for parser tokens, strings, and stacks, removing ambient packet-scope dependence and deleting long-dead parser code. |
| !4192 | closed | Discussion-focused (superseded) | Draft build-directory whitespace fixes became partly stale as CMake code changed. John Thacker later carried only the still-relevant quoting fix in !12256. Useful evidence to re-evaluate a patch against current code rather than preserve obsolete hunks. |
| !4191 | merged | Scanned | PFCP Rule ID presentation improvement; no new durable convention. |
| !4190 | merged | Scanned | TWAMP Request-Session decoding adds schedule-slot and packet-count fields. |
| !4189 | merged | Discussion-focused | QUIC Follow Stream checks absent stream/map state before lookup when keys are unavailable. Stig Bjørlykke immediately exposed separate 32-bit pointer/integer portability failures, later resolved more completely by !4686. |
| !4188 | merged | Scanned | TWAMP Request-Session decoding adds Conf-Sender/Conf-Receiver fields. |
| !4187 | merged | Scanned | MPLS labels can be displayed in hexadecimal; presentation option. |
| !4186 | merged | Deep | Evan Huus adds explicit allocator parameters to OSI helpers instead of relying on global packet scope; part of the explicit-lifetime API series. |
| !4185 | merged | Deep | WSLua replaces ambient packet-scope allocations with pinfo->pool where available or explicit allocate/free paths otherwise. |
| !4184 | merged | Scanned | O-RAN extension parsing checks actual dissected length against the declared extension length. |
| !4183 | merged | Scanned | User Guide documents changed byte-view hover behavior. |
| !4182 | merged | Discussion-focused | BGP-LS Flex Algorithm support shipped with focused sample captures; Alexis La Goutte corrected terminology and a display-filter typo before merge. |
| !4181 | merged | Scanned | CI output tweak for Clang analyzer artifacts; no durable convention beyond keeping debug/diagnostic commits intentional. |
| !4180 | merged | Scanned | Removes duplicate apt command in daily GitLab CI. |
| !4179 | closed | Discussion-focused (superseded) | Early TWAMP Conf-Sender/Receiver change received Alexis La Goutte guidance to use proto_tree_add_item(); later clean implementation merged as !4188. |
| !4178 | merged | Discussion-focused | Qt byte-view hover behavior becomes configurable; review clarified hover versus selection semantics and later documentation/persistence follow-up. |
| !4177 | merged | Discussion-focused | Exported-PDU name-resolution UI checks semantic packet endpoint address types rather than requiring an IP protocol-layer node; Roland Knall also requested a commit description explaining intent. |
| !4176 | merged | Deep | John Thacker tightens the VSS Monitoring trailer heuristic: weak port-stamp-only recognition is opt-in, while the default requires stronger timestamp evidence. Master-origin evidence for confidence-driven heuristic defaults. |
| !4175 | merged | Discussion-focused | Gerald Combs migrates PNG compression tooling from Bash/Make to Python for portability; follow-up discussion suggests validating changed PNGs in commit checks. |
| !4174 | merged | Scanned | Adds BGP-LS RFC 9104 Extended Administrative Groups. |
| !4173 | merged | Scanned | Automatic data/translation update for master-3.2. |
| !4172 | merged | Scanned | Automatic data/translation update for release-3.4. |
| !4171 | merged | Scanned | Automatic data/translation update for master. |
| !4170 | merged | Deep | Roland Knall required an unrelated compile-disabled 'Jackpot' mode to be split from the related-packet refactor and warned that code excluded by default will rot unless it has a supported build/CI path. The author narrowed the MR before merge. |
| !4169 | merged | Deep | Guy Harris makes BLF failures set explicit Wiretap error categories and err_info: malformed file, unsupported feature, or internal failure, rather than returning FALSE with err==0 and accidentally looking like EOF. |
| !4168 | merged | Deep | Guy Harris distinguishes normal EOF at the start of the next record from EOF after a record has begun; subsequent reads convert an otherwise bare EOF into WTAP_ERR_SHORT_READ. |
| !4167 | merged | Scanned | Guy Harris removes redundant function names from ws_debug() messages because wslog already supplies file/line/function context. |
| !4166 | merged | Scanned | Guy Harris centralizes more record metadata initialization in blf_init_rec(). |
| !4165 | merged | Scanned | Guy Harris factors repeated BLF log-object-header reading/endianness conversion into one helper. |
| !4164 | merged | Scanned | Guy Harris fixes a malformed CMake uninstall script line. |
| !4163 | merged | Scanned | Guy Harris fixes misleading loop indentation in BLF. |
| !4162 | merged | Scanned | Guy Harris makes blf_read_block() static because it is file-local. |
| !4161 | merged | Scanned | Stable-branch Windows OS-version display update; cherry-pick with no additional review lesson. |

## High-authority evidence in this batch

Guy Harris directly authored MRs !4202, !4169, !4168, !4167, !4166, !4165, !4164, !4163, and !4162. The strongest reusable guidance is in !4202 (generated-source ownership plus Ethernet trailer boundary) and !4168/!4169 (Wiretap EOF/error semantics). Those are treated as very high-confidence conventions.

John Thacker authored merged !4206 and !4176, supplying strong framework-level dispatch and heuristic-default evidence. Evan Huus authored the explicit-allocation-scope series !4193, !4194, !4185, and !4186.

Closed !4209, !4192, and !4179 are deliberately not treated as accepted implementation precedent. Their useful review feedback is retained only as lower-weight workflow, licensing, API-reuse, or supersession evidence.
