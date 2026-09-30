# Review findings: Wireshark MRs !3511–!3560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is treated as stronger implementation precedent than closed or superseded work. Direct maintainer guidance—especially Guy Harris—receives additional weight. Every row below corresponds to one MR in the exact run ledger.

| MR | Outcome | Review result |
|---|---|---|
| !3560 | merged | Automatic release-3.2 registry/translation refresh. Routine generated-data maintenance; no new durable convention. |
| !3559 | merged | Pascal Quantin fixes NR RRC `MeasTriggerQuantityOffset` display semantics in the ASN.1 configuration/template and regenerated C. The encoded integer is a half-dB offset, so a custom formatter is used instead of misleading absolute-unit display metadata. |
| !3558 | merged | Automatic release-3.4 registry/translation refresh. No durable engineering-review lesson. |
| !3557 | merged | Automatic master registry/translation refresh. No durable engineering-review lesson. |
| !3556 | merged | **Strong Guy Harris review.** João Valverde moves version-info code out of EPAN toward UI layering, but Guy catches that the first revision silently removes zlib from `editcap --version`. Accepted code preserves the observable dependency/version diagnostic. Guy also sketches a set-based deduplication model for version strings. Layering refactors must preserve user-visible capability diagnostics. |
| !3555 | merged | TECMP CAN/FlexRay/LIN identifiers switch to `BASE_HEX_DEC` where seeing both representations is useful. |
| !3554 | merged | Martin Mathieson fixes/updates O-RAN FH CUS section-extension handling. Protocol-specific maintenance; no new general rule promoted. |
| !3553 | merged | ASTERIX fixes the displayed Data Item number for 010/091. Straightforward field-label correctness. |
| !3552 | merged | F1AP upgraded to 3GPP TS 38.473 v16.6.0 by updating ASN.1/conformance inputs and regenerated output together. Corroborates generated-source discipline. |
| !3551 | merged | E1AP upgraded to 3GPP TS 38.463 v16.6.0 from ASN.1 source with regenerated output. Corroborating generated-code evidence. |
| !3550 | merged | ASTERIX corrects the VY display label that incorrectly said VX. |
| !3549 | merged | XnAP upgraded to 3GPP TS 38.423 v16.6.0 from ASN.1/conformance sources and regenerated output. |
| !3548 | merged | NGAP upgraded to 3GPP TS 38.413 v16.6.0 from ASN.1/conformance sources and regenerated output. |
| !3547 | merged | X2AP upgraded to 3GPP TS 36.423 v16.6.0 from ASN.1/conformance sources and regenerated output. |
| !3546 | merged | S1AP upgraded to 3GPP TS 36.413 v16.6.0 from ASN.1 source and regenerated output. |
| !3545 | merged | Signal PDU improves UAT validation/naming/descriptions. Useful configuration hygiene, but no distinct general rule beyond existing UAT guidance. |
| !3544 | merged | Accepted Kerberos-without-Kerberos build fix, superseding !3536. Kerberos-independent field registrations are kept outside `HAVE_KERBEROS`. Review also records normal rebase/commit-message cleanup and a transient GitLab REST 502 diagnosed by Gerald Combs. |
| !3543 | merged | Adds basic PAC_TICKET_CHECKSUM dissection. Stefan Metzmacher asks about eventual verification and checksum input semantics; useful protocol-review context but no new broad convention. |
| !3542 | merged | DNP3 shows octet-string length in the object item because variation encodes length for that object family. |
| !3541 | merged | Gerald Combs makes Win32 UI sources explicitly C++ (matching how MSVC already compiled them) and converts affected allocations to C++ ownership. |
| !3540 | merged | O-RAN FH CUS handles the special `numPrbu == 0` meaning and related display/configuration details. |
| !3539 | merged | Anders Broman first asks whether `BASE_HEX_DEC` suffices, then catches the need for the portable GLib 64-bit format modifier. Because Signal PDU values can have arbitrary protocol bit widths, fixed storage-width hex formatting can be misleading; explicit formatting is justified when presentation must reflect semantic bit width. |
| !3538 | merged | **Strong heuristic guidance from Peter Wu and Pascal Quantin.** WebSocket's TCP heuristic is registered disabled by default using Wireshark's built-in `HEURISTIC_DISABLE` mechanism instead of a duplicate preference. Peter quantifies false-positive risk; review also recognizes that TCP segmentation makes naive whole-frame length validation unsafe. |
| !3537 | merged | Adds a Signal PDU preference controlling whether raw-value fields are hidden, preserving the old behavior by default. |
| !3536 | closed | Earlier Kerberos no-library build fix. Superseded by merged !3544 and down-weighted. |
| !3535 | merged | **Guy Harris-authored.** Adds ZigBee NWK/APS pcapng secrets types to the shared Wiretap secrets-type definitions because those identifiers are defined by the format specification. |
| !3534 | merged | MP2T reassembly-table function table made `static const`; ordinary linkage hygiene. |
| !3533 | merged | **Guy Harris-authored.** Corrects Wiretap option documentation: custom string options are specifically UTF-8 strings, distinct from binary custom options. |
| !3532 | merged | **Guy Harris-authored.** Adds a dedicated `WTAP_BLOCK_SYSDIG_EVENT` block type for future Wiretap use. |
| !3531 | merged | Radiotap adds the standardized Data Retries field; Richard Sharpe reviewed it positively. |
| !3530 | merged | John Thacker regenerates LDAP from ASN.1 after a source/template line-number change so checked-in generated output remains reproducible, even though the semantic generated change is only a line marker. |
| !3529 | merged | John Thacker ensures DVB-S2 Mode Adaptation is represented in the tree even for zero-byte L.1 headers so protocol preferences remain reachable. A logical protocol layer need not consume bytes to have registration/presentation state. |
| !3528 | merged | TLS delegated-credentials support merged against draft-era semantics and a supplied capture. A later John Thacker review notes the interpretation is wrong for final RFC 9345 and asks how the capture was produced; a later successor was proposed. Treat this as cautionary evidence to revalidate draft-era protocol code and capture provenance against the final standard. |
| !3527 | merged | WebSocket is registered for TCP Decode As in addition to HTTP Upgrade dispatch. |
| !3526 | merged | VSS has no active preferences, so its preference module becomes `prefs_register_protocol_obsolete()` while the old `use_heuristics` key remains recognized as obsolete. |
| !3525 | merged | SPNEGO decodes `mechListMIC` through the selected GSS mechanism; Isaac Boukris suggests simplifying the ASN.1 conformance directive. Source/conformance input remains the maintenance surface. |
| !3524 | closed | Duplicate/superseded Radiotap Data Retries submission; merged !3531 is the implementation precedent. |
| !3523 | merged | O-RAN starts Section Extension 11 support and updates extension-name registry. Protocol-specific groundwork. |
| !3522 | merged | Kerberos PAC verification broadens the server-checksum key search for U2U, but Stefan Metzmacher correctly narrows the change so KDC checksum verification still uses the long-term-key domain. Broaden fallback domains only for the semantic role that requires them. |
| !3521 | merged | PCEP implements ASSOC-Type-List TLV using IANA/RFC values and supplies a sample capture. Good protocol submission evidence; no separate new convention needed. |
| !3520 | merged | VoIP call graph code now checks that the graph-analysis sink exists before allocating a sequence-analysis item, removing a leak. Acquire resources only after the precondition that gives them an owner/consumer is known true. |
| !3519 | merged | JSON path filtering received extensive CI/compiler cleanup. Anders Broman catches trailing whitespace and `-Werror=shadow`; the contributor correctly avoids rewriting unfamiliar behavior solely to silence a likely checker false positive. Fix real diagnostics, but do not distort semantics to satisfy a mistaken checker. |
| !3518 | merged | PROFINET moves a `break` so the submodule scan stops only after the PROFIsafe module is actually found. Repeated searches must terminate on the semantic match, not merely the first candidate. |
| !3517 | merged | **Guy Harris-authored.** pcapng option-size/write helpers take the full `wtap_optval_t` and choose the correct union member centrally. Callers no longer encode knowledge of union representation. |
| !3516 | merged | Gerald Combs scopes dedicated Windows-runner CI rules to the main `wireshark/wireshark` project, while still allowing explicit web runs. Pipeline type alone is insufficient when runner availability is repository-scoped. |
| !3515 | merged | Jaap Keuter cannot cherry-pick the ASTERIX fix to release-3.4 because an unrelated whitespace edit is mixed into the commit, explicitly calling it another example of why mixing changes is bad form. Keep stable-worthy fixes mechanically focused and backportable. |
| !3514 | merged | OSPF corrects SRLB/SRMS TLV type values from RFC 8665, improves unknown-TLV display, and supplies a capture. Strong spec-plus-reproducer practice, but existing notebook guidance already covers it. |
| !3513 | merged | MP2T marks a local as `volatile` where setjmp/longjmp semantics otherwise trigger a clobber warning under Werror. |
| !3512 | merged | SMB2 read response parsing reuses the existing offset/length-buffer abstraction and explicitly models its reserved/padding byte, rather than open-coding a one-off data offset. |
| !3511 | merged | **Guy Harris-authored.** pcapng helpers are renamed consistently as `pcapng_compute_XXX_option_size()` and `pcapng_write_XXX_option()` and organized in option-type order. Function names should expose the semantic role consistently across a family. |

## Durable themes promoted

The strongest reusable themes were promoted to the topical notebook files for library layering, heuristics, generated source, pcapng option architecture, preference migration, CI job scope, parser progress, resource-allocation preconditions, stable-backport scope, and protocol-specification validation.

No SMPTE 291/VANC packet type was encountered in this batch.
