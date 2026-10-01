# Review findings — !2711–!2760

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are treated as accepted evidence. Closed !2714 is retained only as lower-weight historical context and is explicitly compared with later merged successor !4787.

| MR | Review | Findings |
|---|---|---|
| !2760 | Scanned | Release-note update for the RTP/VoIP UI redesign; documentation-only. |
| !2759 | Scanned | Automatic maintained-branch data/AUTHORS/NEWS update; no durable engineering rule. |
| !2758 | Scanned | Automatic release-3.4 data/AUTHORS/NEWS update; no durable engineering rule. |
| !2757 | Scanned | Automatic master translation/data update; no durable engineering rule. |
| !2756 | Deep | Martin Mathieson clears the STUN address object before parsing each attribute, preventing a partially populated or unsupported variant from retaining state from an earlier attribute. |
| !2755 | Scanned | AUTOSAR NM expands Control Bit Vector version support with explicit semantic fields and reserved bits. |
| !2754 | Deep | USBLL replaces a loosely accumulated packet-state model with an explicit transaction state machine covering valid and invalid PID sequences. |
| !2753 | Deep | Martin Mathieson fixes copy/paste errors in several display-filter abbreviations, including collisions and semantically wrong names; reinforces global filter-namespace accuracy. |
| !2752 | Scanned | Adds IEEE 802.11 QoS Management attributes; protocol feature work. |
| !2751 | Deep | Pascal Quantin requires named SOME/IP dissector handles to be registered in `proto_register_someip()`, before handoff; he explains that registry identity is used by other consumers such as exported decrypted PDUs. |
| !2750 | Scanned | Adds the DSCP Policy Query value to the IEEE 802.11 QoS subtype table. |
| !2749 | Scanned | Splits one packed spatial-stream allocation into two semantic fields, improving filterability and meaning. |
| !2748 | Scanned | Adds Wi-Fi QoS Management V2 action support and shared action dispatch. |
| !2747 | Deep | Corrects several field registrations whose `FT_*` types were narrower or otherwise inconsistent with the values actually dissected. |
| !2746 | Scanned | Qt missing-prototype cleanup; makes externally visible C-linkage entry points explicitly declared. |
| !2745 | Deep | Epan missing-prototype cleanup updates ASN.1 templates and generated dissector C together, reinforcing source/generator parity. |
| !2744 | Scanned | Moves shared Bluetooth BR/EDR RF state/API definitions into a header used by both dissectors. |
| !2743 | Scanned | Adds missing plugin registration prototypes; build-warning cleanup. |
| !2742 | Deep | Wiretap warning cleanup makes translation-unit-only helpers `static`, reducing accidental external linkage/API surface. |
| !2741 | Deep | Adds Lemon parser prototypes in the `.lemon` sources, not merely generated output; reinforces generator-source ownership. |
| !2740 | Scanned | CI moves from Clang 11 to Clang 12. |
| !2739 | Scanned | Uses a custom display formatter for encoded STS values whose user-visible value is raw+1. |
| !2738 | Scanned | Corrects a typo in an expert-field filter abbreviation introduced earlier. |
| !2737 | Scanned | Adds BGP SRv6 uSID draft support; protocol feature work. |
| !2736 | Deep | LDAP SASL stable-branch fix moves decrypted-payload dissection outside the optional tree-building block, so semantic dissection occurs even with no tree; template and generated C remain synchronized. |
| !2735 | Deep | Windows CI exposes collisions between Wireshark's generic `REG_*` macros and WinNT.h pulled in transitively. Pascal Quantin traces the include chain; accepted code guards matching definitions. |
| !2734 | Scanned | Stable-branch LDAP fix makes the SASL subtree span the payload and exclude the four-byte length prefix. |
| !2733 | Scanned | SMB2 protocol updates; Pascal catches a wording typo during review. |
| !2732 | Scanned | Release-3.4 counterpart of the LDAP SASL payload-boundary correction. |
| !2731 | Scanned | Adds RTPS UDPv4 WAN transport elements. |
| !2730 | Deep | Master LDAP SASL regression fix; same important tree-independent semantic-dissection behavior as !2736, with ASN.1 template/generated output parity. |
| !2729 | Scanned | Adds NAS 5GS operator-defined access category definitions. |
| !2728 | Deep | Conflict-resolution MR repairs several duplicate or wrong filter abbreviations; Jim Young spots an additional typo and Pascal points to follow-up !2738. |
| !2727 | Deep | GTPv2 configuration-transfer support changes ASN.1 configuration and generated outputs together. Pascal also reiterates that Gerrit `Change-Id` trailers are obsolete under GitLab. |
| !2726 | Deep | Guy Harris stable-branch fix computes the old ptvcursor copy length in bytes before increasing capacity; fixes a deep-subtree crash. |
| !2725 | Deep | Guy Harris release-3.4 counterpart of the ptvcursor byte-count fix. |
| !2724 | Deep | Guy Harris simplifies the master ptvcursor growth path to `wmem_realloc()`, eliminating the manual allocate/copy path that made the byte-count bug possible. |
| !2723 | Scanned | Adds IS-IS RFC 8570 TE metric extensions. |
| !2722 | Deep | TCP graph fixes reset all associated plottables and distinguish window updates from duplicate ACKs before plotting. |
| !2721 | Scanned | Uses a `val64_string` table for encoded LTF-total semantics. |
| !2720 | Deep | RTP Player crash fix locks the UI around mutations that can race with active playback; reporter confirms the crash is fixed. |
| !2719 | Deep | Large VoIP/RTP UI refactor increasingly passes stable `rtpstream_id_t` identities between dialogs rather than mutable stream snapshots. |
| !2718 | Deep | Gerald Combs fixes RTP hashing by assigning the return from `add_address_to_hash()`; scalar accumulators passed by value do not mutate unless the returned value is propagated. |
| !2717 | Deep | Anders Broman suggests `BASE_HEX_DEC` when adding decimal presentation to a field users are accustomed to seeing in hex; accepted change preserves both views. |
| !2716 | Scanned | Adds the Last Used E-UTRAN PLMN ID to BSSMAP Common ID parsing. |
| !2715 | Scanned | Maintained-branch conversation output explicitly handles epoch timestamp presentation. |
| !2714 | Closed / down-weighted | Proposed suppressing a TCP desegmentation assertion and silently returning when reassembly is unavailable. Later merged !4787 by John Thacker establishes the stronger behavior: throw `FragmentBoundsError` because requested bytes are unavailable, without classifying it as a dissector programming assertion. |
| !2713 | Deep | Signal PDU allocated two dynamic fields per configuration row but recorded only `data_num`; fix makes registered-count metadata equal the actual `2 * data_num` population. |
| !2712 | Scanned | AUTOSAR NM makes CBV/SNI byte positions independently configurable, including disabled. |
| !2711 | Deep | Guy Harris initializes aggregate exit status before the interface loop, breaks on first failure, and removes the unconditional post-loop success assignment so failures survive shared cleanup. |

No SMPTE ST 291/VANC packet type was encountered in this batch.
