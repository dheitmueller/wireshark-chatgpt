# Review findings — Wireshark MRs !2761–!2810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Evidence weighting: merged changes are primary evidence; closed submissions are historical only. The corpus snapshots contain no non-system human discussion notes for these fifty MRs, so maintainer authority is reflected primarily by authorship and accepted implementation. Guy Harris-authored !2762 is the strongest architectural exemplar in this batch; John Thacker-authored !2807 and !2798 are also weighted strongly.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !2810 | merged | Scanned | Automatic translations, registries, vendor data, AUTHORS and release-note refresh; no durable new convention. |
| !2809 | merged | Deep | Corrects mismatches among wire widths, item lengths, and registered `FT_UINT*` widths in PPCAP, Tibia, and TNEF. |
| !2808 | merged | Scanned | Bluetooth Mesh PDU naming typo only. |
| !2807 | merged | Scanned | John Thacker updates BGP SAFI values from the IANA registry; registry-maintenance corroboration. |
| !2806 | merged | Deep | CAN IDs share storage with flag bits; accepted code masks with `CAN_EFF_MASK` before matching and hashing logical IDs. |
| !2805 | merged | Scanned | Qt HiDPI policy adjustment for Windows. |
| !2804 | merged | Scanned | AJP13 presentation improvement for common headers and request attributes. |
| !2803 | merged | Deep scan | Reuses the existing STS custom formatter so the same encoded value has consistent semantic display. |
| !2802 | merged | Scanned | Initializes an NVMe tree pointer to satisfy warning-clean builds across compilers. |
| !2801 | merged | Deep | RTP Player live-capture refactor passes stable `rtpstream_id_t` identities between components and computes mutable statistics locally. |
| !2800 | merged | Scanned | Narrows symbol scope by making variables static. |
| !2799 | merged | Scanned | Adds DCERPC TaskSchedulerService operation-name mapping. |
| !2798 | merged | Deep | John Thacker gives the shared E.212 decoder ECGI context so MCC/MNC fields get the right semantic identity. |
| !2797 | merged | Scanned | Updates 802.11 transmit-power-envelope terminology to match current IEEE usage. |
| !2796 | merged | Scanned | Release version bump only. |
| !2795 | merged | Scanned | Release version bump only. |
| !2794 | merged | Scanned | Pascal Quantin adds an explicit float cast required by MSVC; portability corroboration. |
| !2793 | merged | Scanned | Release build metadata only. |
| !2792 | merged | Scanned | Release build metadata only. |
| !2791 | merged | Deep | PASN parsing switches from hand-picked optional IE combinations to repeatedly invoking the generic tagged-field parser for the remaining IE sequence. |
| !2790 | closed | Down-weighted | Attempted Qt/CI fix; closed without merge or substantive discussion. |
| !2789 | merged | Scanned | Removes incorrect statistics-table writes that targeted the wrong column. |
| !2788 | merged | Scanned | RSerPool statistics documentation only. |
| !2787 | merged | Deep | PTP signalling proves the full fixed TLV Length+Type header fits inside the declared message before reading either field. |
| !2786 | merged | Deep | GSMTAP uses `BASE_UNIT_STRING` with shared dB/dBm unit metadata instead of embedding units in field labels. |
| !2785 | merged | Deep | ICMP extension placement prefers explicit original-datagram length and uses the historical 128-byte location only as compatibility fallback. |
| !2784 | merged | Scanned | Release-note preparation only. |
| !2783 | merged | Scanned | Release-note preparation only. |
| !2782 | merged | Backport | Release-3.2 backport of !2766 count/allocation hardening. |
| !2781 | merged | Backport | Release-3.4 backport of !2766 count/allocation hardening. |
| !2780 | merged | Scanned | Adds RTPS coherent-set PIDs. |
| !2779 | merged | Deep | Accepted GSM MAP typo fix changes both authoritative ASN.1 and generated dissector C. |
| !2778 | merged | Scanned | Extends NVMe Get Log Page response decoding; protocol-specific. |
| !2777 | closed | Down-weighted | Earlier closed GSM MAP typo submission; merged !2779 is the accepted precedent. |
| !2776 | closed | Down-weighted | Earlier closed GSM MAP typo submission; merged !2779 is the accepted precedent. |
| !2775 | merged | Scanned | GitLab issue-template quick-action metadata. |
| !2774 | merged | Scanned | Spelling cleanup and dictionary update. |
| !2773 | merged | Scanned | tshark documentation correction. |
| !2772 | merged | Scanned | Broad editor-modeline cleanup. |
| !2771 | merged | Scanned | RTP UI cleanup; no stronger rule than !2801. |
| !2770 | merged | Backport | CMake AUTOUIC/AUTOMOC/AUTORCC compatibility logic for older CMake on master-3.2. |
| !2769 | merged | Backport | Same CMake compatibility fix for release-3.4. |
| !2768 | merged | Scanned | Qt const/missing-prototype warning cleanup. |
| !2767 | merged | Backport | Qt string-width consistency fix across Qt versions. |
| !2766 | merged | Deep | Gerald Combs bounds packet-controlled counts before large MS-WSP allocations and still verifies required bytes. |
| !2765 | merged | Scanned | VoIP Calls and RTP Streams selection integration. |
| !2764 | merged | Deep | UAVCAN separates CAN transport/framing from DSDL message semantics so DSDL can be reused by future transports; large source bodies are collapsed in this corpus snapshot. |
| !2763 | merged | Scanned | NVMe spelling cleanup. |
| !2762 | merged | Deep / high authority | Guy Harris gives CommView NCF and NCFX distinct Wiretap openers and extension identities. |
| !2761 | merged | Scanned | Preference to disable packet-list hover colorization. |

No SMPTE ST 291/VANC packet type was encountered.
