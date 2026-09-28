# Wireshark MR findings 5661-5710
Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Merged work is primary evidence. MR 5685 was open and MR 5671 closed, so both are down-weighted.

| MR | Outcome | Finding |
|---|---|---|
| 5710 | merged | GTPv2 accepts missing Diameter ULI context and passes NULL to the shared decoder rather than dereferencing opaque data. |
| 5709 | merged | NGAP v16.8 ASN.1 sources/templates and generated dissector remain synchronized. |
| 5708 | merged | NR RRC v16.7 source ASN.1/conformance data and generated output stay synchronized. |
| 5707 | merged | Display-filter byte literals accept contiguous hexadecimal without separators. |
| 5706 | merged | X2AP v16.8 source ASN.1, conformance registrations, template, and generated output move together. |
| 5705 | merged | S1AP v16.8 source and generated artifacts move together, including a new NR paging dependency. |
| 5704 | merged | John Thacker makes text2pcap use the common -F output-format contract shared by tshark/editcap/mergecap and updates docs/release notes. |
| 5703 | merged | LPP v16.7 source ASN.1 and generated output stay synchronized. |
| 5702 | merged | LTE RRC v16.7 source ASN.1/templates and generated output stay synchronized. |
| 5701 | merged | Signal-PDU moves a bounds check before allocation, removing an early-return leak. |
| 5700 | merged | Gerald Combs centralizes hover color policy in ColorUtils with platform/theme-aware behavior. |
| 5699 | merged | Gerald Combs removes decorative alternating rows because Wireshark already uses row color semantically. |
| 5698 | merged | UAT missing-column compatibility now checks default_values before indexing, fixing a crash. |
| 5697 | merged | Signal-PDU adds DLT subdissector support and publishes the needed DLT context/constants. |
| 5696 | merged | release-3.4 backport of the Cisco IKEv2 VID additions. |
| 5695 | merged | release-3.6 backport of the Cisco IKEv2 VID additions. |
| 5694 | merged | Automatic release-3.6 generated/documentation update; no new durable convention. |
| 5693 | merged | Automatic master generated/documentation update; no new durable convention. |
| 5692 | merged | Automatic release-3.4 generated/documentation update; no new durable convention. |
| 5691 | merged | BLF reads interface names from application-text metadata into Wiretap interface blocks. |
| 5690 | merged | MPEG registration descriptor keeps formal uint32 identity while adding readable registered-ID/organization mapping after Anders Broman review. |
| 5689 | merged | João Valverde requires separators for ISO 8601 filter times so all-numeric basic form remains available for future epoch-second syntax. |
| 5688 | merged | Master implementation recognizing additional Cisco IKEv2 VIDs; 5695/5696 are stable backports. |
| 5687 | merged | License tooling names BSD-1-Clause precisely instead of generic BSD. |
| 5686 | merged | DVB EIT schedule table IDs expanded and named. |
| 5685 | open | Lower-weight review history: reviewers raised license proliferation, UI semantics, common theme colors, zebra striping, and squash/rebase concerns; 5700 and 5699 separately merged the color-policy results. |
| 5684 | merged | text2pcap gains regex import mode, moving CLI import toward GUI feature parity. |
| 5683 | merged | João Valverde reverts epan-owned Wiretap init/cleanup because it crashes on exit; explicit Wiretap-before-libwireshark ordering remains. |
| 5682 | merged | release-3.4 annual copyright update. |
| 5681 | merged | release-3.6 annual copyright update. |
| 5680 | merged | Guy Harris flags duplicated Windows resource copyright strings; Gerald Combs agrees common ownership may be preferable. |
| 5679 | merged | Developer docs remove obsolete Buildbot references in favor of GitLab CI. |
| 5678 | merged | Gerald Combs enables Windows UTF-8 active code page; Guy Harris explicitly checks older-Windows fallback requirements. |
| 5677 | merged | João Valverde normalizes CMake flags to positive ENABLE semantics; Alexis La Goutte requests and receives release-note coverage. |
| 5676 | merged | Bluetooth device-class parser updated for LE Audio. |
| 5675 | merged | Import-from-Hex-Dump persists interface name, completing the runtime UI change from 5666 after review found the omission. |
| 5674 | merged | Mechanical repeated-word cleanup. |
| 5673 | merged | Arch setup gains DocBook requirements for guide generation. |
| 5672 | merged | Documentation clarifies any/all equality semantics for repeated display-filter fields. |
| 5671 | closed | Lower-weight diagnostic draft only; John Thacker records a Windows pytest/logging abort needing separate investigation. |
| 5670 | merged | João Valverde makes ISO 8601 the default absolute-time filter representation while retaining legacy input; John Thacker identifies numeric-basic ambiguity that leads to 5689. |
| 5669 | merged | release-3.6 backport of the timezone-sign correction. |
| 5668 | merged | John Thacker fixes reversed timezone arithmetic and corrects tests that encoded the same misconception. |
| 5667 | merged | NTLMv2 AUTH parsing skips target-info fields that AUTHENTICATE messages do not carry. |
| 5666 | merged | GUI import gains interface-name output; Stig Bjørlykke catches missing settings persistence, fixed in 5675. |
| 5665 | merged | DVB/HEVC descriptor text updated to newer BlueBook definitions. |
| 5664 | merged | John Thacker moves SHB/IDB preparation to shared text_import code and documents caller ownership of allocated IDB information. |
| 5663 | merged | Jaap Keuter says clang-check discovery should skip all packet template C files, not one protocol-specific template exception. |
| 5662 | merged | X.509 RFC 7468 public-key support updates conformance/template source and generated dissector together. |
| 5661 | merged | macOS text-import address fields get the same minimum-height handling as sibling fields. |
