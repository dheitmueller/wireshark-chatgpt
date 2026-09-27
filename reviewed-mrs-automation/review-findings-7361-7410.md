# Review findings: !7361-!7410

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed merge requests. Merged work is primary evidence; closed or superseded work is down-weighted and used only where its review history is independently useful.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !7410 | merged | Scanned | João Valverde corrected the display-filter VM text so ALL and ANY equality and inequality operations show their distinct operators. |
| !7409 | closed/superseded | Discussion | Alexis La Goutte required a dedicated topic branch instead of the contributor's master branch; the change was resubmitted and merged as !7416. |
| !7408 | merged | Discussion | IPv6 NAT64 recognition became user-configurable through a UAT instead of relying on a fixed prefix assumption. |
| !7407 | merged | Scanned | 802.11 broken-frame-control handling separated the byte-swap display offset from the real frame-control base offset, avoiding incorrect flag decoding. |
| !7406 | merged | Discussion | Gerald Combs added the Qt5Concurrent development package needed on openSUSE after CI exposed the missing dependency; John Thacker followed up for SUSE packaging. |
| !7405 | merged | Deep | Anders Broman rejected putting GRE-specific state into packet_info. The accepted design passed a typed GRE context structure containing flags and the GRE key through the subdissector data argument. |
| !7404 | merged | Scanned | IEEE 802.11 VHT channel-width values were aligned with the current standard and the shared channel-width value table. |
| !7403 | merged | Scanned | tshark quiet mode now suppresses non-error capture-start and file messages. |
| !7402 | merged | Discussion | Build and packaging call sites invoke the Python versioning tool in a platform-appropriate way rather than assuming the same interpreter command everywhere. |
| !7401 | closed/superseded | Discussion | Uli Heilmeier rejected distro-name branching where the setup script already probes package availability; absence of openSUSE-only packages on another RPM distribution is not a hard failure. Superseded by !7474. |
| !7400 | merged | Scanned | The X.509 RDN display buffer limit was increased for real-world names; no broader convention was extracted. |
| !7399 | closed | Scanned | RTPS optional-member parsing experiment was not accepted; no implementation precedent was promoted. |
| !7398 | closed/superseded | Discussion | John Thacker explained that XML 1.0 cannot represent most C0 controls and recommended a visible textual escape while preserving valid UTF-8. Later merged !8184 is stronger authority. |
| !7397 | merged | Deep | Guy Harris replaced x86-only CPUID brand strings with OS-native CPU description mechanisms so diagnostics work across instruction sets and operating systems. |
| !7396 | merged | Discussion | Address-resolution profile cleanup now closes the old VLAN file before clearing cached state, preventing stale names and file handles across profile switches. |
| !7395 | merged | Scanned | SOME/IP removed obsolete legacy datatype support after Signal-PDU became the supported path and tightened related UAT validation. |
| !7394 | merged | Discussion | Missing-prototype cleanup made private functions static or added proper declarations; a subsequent generated-header CI issue was fixed rather than weakening the warning. |
| !7393 | merged | Discussion | John Thacker traced an ABI-check failure to a known abi-dumper and GCC incompatibility; the build container was reverted instead of changing correct code to satisfy a broken checker. |
| !7392 | merged | Scanned | Automatic registry and data update. |
| !7391 | merged | Scanned | Automatic registry, data, and translation update. |
| !7390 | merged | Scanned | Automatic manufacturer and service registry update. |
| !7389 | merged | Scanned | Stable backport of the EVPN Router's MAC wording correction. |
| !7388 | merged | Scanned | LAPD initializes direction-specific byte state before first use when conversation state exists but the side-specific state pointer is still null. |
| !7387 | closed/superseded | Scanned | Apple-Silicon-specific CPU-brand handling was superseded by Guy Harris's broader merged !7397 design. |
| !7386 | merged | Scanned | pcapng specification links were updated to the current IETF draft location. |
| !7385 | merged | Scanned | MSVC permissive-mode handling is enabled only for Qt versions that require it, keeping the compatibility workaround version-scoped. |
| !7384 | merged | Discussion | Display-filter expression item generation and sorting moved to QtConcurrent so the dialog can appear promptly; Roland Knall noted a model/view refactor would be cleaner long term. |
| !7383 | closed/superseded | Discussion | Earlier event-loop and sorting cleanup was folded into merged !7384 and was not treated separately as accepted precedent. |
| !7382 | merged | Deep | John Thacker centralized resolved-versus-unresolved custom-column selection in get_column_text and converted GUI, command-line, export, print, and search consumers away from raw column storage. |
| !7381 | merged | Scanned | Master EVPN Router's MAC wording correction. |
| !7380 | merged | Discussion | HTTP/2 adds a filterable full request URI derived from scheme, authority, and path rather than leaving those semantics only in separate pseudo-headers. |
| !7379 | merged | Scanned | Qt traffic-tree proxy members are explicitly initialized, removing undefined initial state reported by static analysis. |
| !7378 | merged | Scanned | TECMP configurable control-message IDs moved into a UAT where the protocol permits user-defined values. |
| !7377 | merged | Discussion | John Thacker fixed multifield custom-column handling so updating one unresolved field segment does not overwrite the entire composite value. |
| !7376 | merged | Scanned | BACnet registry and optional-field decoding corrections. |
| !7375 | merged | Scanned | Keysight/Ixia NetFlow vendor-field update after naming and description review. |
| !7374 | merged | Deep | The make-version Perl-to-Python port was reviewed as a behavior-preserving migration: Gerald Combs checked exact mode semantics and the change updated all build, packaging, documentation, and CI call sites. |
| !7373 | merged | Scanned | Debian symbol list updated for a newly exported wsutil symbol. |
| !7372 | merged | Deep | Guy Harris changed NHRP extension tree construction to begin open-ended and finalize the item extent after parsing, so malformed declared lengths fail at the actual field access rather than before useful dissection. |
| !7371 | merged | Deep | Guy Harris changed NHRP Vendor ID from a byte-string field to FT_UINT24 because the dissector semantically extracts a three-octet unsigned integer. |
| !7370 | merged | Deep | Stable-branch counterpart of Guy Harris's NHRP extension-boundary cleanup, corroborating the same parser rule. |
| !7369 | merged | Discussion | Stable-branch counterpart of the NHRP FT_UINT24 Vendor ID correction. |
| !7368 | merged | Discussion | Alexis La Goutte asked that the reproducer capture be attached to the MR rather than committed as ad-hoc test data; the contributor rebased and supplied it in review. |
| !7367 | merged | Discussion | Another accepted branch of the NHRP extension-boundary cleanup carrying the same Guy Harris design. |
| !7366 | merged | Deep | Guy Harris caught an ASN.1 generator bug where decode width was chosen from range span alone; Pascal Quantin amended the fix so both bounds must also be representable in the narrower type. |
| !7365 | closed/superseded | Scanned | Diameter command-code text-field proposal became unnecessary after equivalent functionality landed elsewhere. |
| !7364 | merged | Discussion | Bluetooth custom UUID labels were modeled as a UAT and exercised with real HCI and pcap captures. |
| !7363 | merged | Scanned | Stable backport of the RTPS union-dissection correction. |
| !7362 | merged | Deep | John Thacker rejected finding per-message HTTP metadata by searching the protocol tree or using conversation state. The accepted HTTP/1 path passes a packet-scope header map through message-info context to the GRPC subdissector. |
| !7361 | merged | Scanned | Spelling cleanup across documentation and code, including correction of an existing misspelled filter abbreviation. |

Highest-confidence reusable evidence from this batch is !7405, !7397, !7382, !7374, !7372/!7370/!7367, !7371/!7369, !7366, and !7362. Closed merge requests are not treated as accepted implementation authority.
