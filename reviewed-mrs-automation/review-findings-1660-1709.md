# Wireshark MR review findings: !1660-!1709

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is weighted above closed/unmerged work. Maintainer review is weighted by authority and specificity.

## Per-MR accounting

- !1709 merged — SOME/IP UTF-16 now carries configured byte order in composable encoding flags; later character-set checks test flag membership and the tree item end is corrected. Promoted to `text-encoding-conventions.md`.
- !1708 merged — GTPv2 Indication IE update; Anders Broman caught trailing whitespace. Corroborates existing pre-submit checks.
- !1707 merged — GPRS CDR TS 32.298 update changes ASN.1 inputs and regenerated C together. Corroborates generated-code source-of-truth guidance.
- !1706 merged — Anders Broman replaces duplicate PFCP bytes/string fields with one `FT_BYTES` field using `BASE_SHOW_ASCII_PRINTABLE`; duplicate abbreviations are also fixed. Promoted.
- !1705 merged — IEEE 802.11 SGDSN ID ANSI support; Alexis La Goutte requested a pcap and the contributor supplied one. Corroborates sample-capture practice.
- !1704 merged — PFCP TS 29.244 update; protocol-specific.
- !1703 merged — Gerald Combs removes an unneeded nstime check; narrow cleanup.
- !1702 merged — Pascal Quantin regenerates NBAP ASN.1 output; routine generated-source maintenance.
- !1701 merged stable — AUTOSAR-NM PNI true/false-string backport; corroborates !1691.
- !1700 merged stable — AUTOSAR-NM PNI true/false-string backport; corroborates !1691.
- !1699 merged revert — reverts a Qt Decode-As delegate change; the reverted implementation is not treated as precedent.
- !1698 closed — direct French Qt translation edit. Dario Lombardo said translations must be fixed in Transifex; Alexis La Goutte applied the correction there. Workflow guidance promoted, implementation down-weighted.
- !1697 merged — EPL reassembly-state fix. Martin Mathieson used `tools/check_typed_item_calls.py` to identify existing width mismatches and described its then-current CI role. Historical checker corroboration.
- !1696 merged — duplicate filter names fixed in ASN.1 conformance inputs plus regenerated output. Corroborates generator/source-of-truth guidance.
- !1695 merged stable — Npcap installer notice backport; packaging-specific.
- !1694 merged — Gerald Combs WiX documentation update; documentation-specific.
- !1693 merged — AUTOSAR-NM range/dispatch cleanup; useful extension-point example but covered by existing guidance.
- !1692 merged — Martin Mathieson fixes repeated `value_string` labels detected by a new checker experiment. Corroborates checker automation.
- !1691 merged — primary AUTOSAR-NM PNI true/false-string correction.
- !1690 merged — OBD-II CAN heuristic is default-disabled because nominal IDs can be reused outside automotive deployments. Promoted to heuristic guidance.
- !1689 merged — TECMP supports explicit and heuristic CAN/FlexRay dispatch with configurable precedence and raw fallback if neither claims the payload. Promoted.
- !1688 merged stable — USBPcap installer link correction.
- !1687 merged — Npcap installer notice; packaging-specific.
- !1686 merged — TShark simple-stat tap private state gains a finish callback; LeakSanitizer exposed the leak. Promoted to tap listener lifecycle guidance.
- !1685 merged — USBPcap installer link correction.
- !1684 merged — GSM A stats removes an always-zero table index.
- !1683 merged — GSM MAP same cleanup; template/generated output remain aligned.
- !1682 merged — ANSI MAP same cleanup; template/generated output remain aligned.
- !1681 merged — CAMEL same cleanup; template/generated output remain aligned.
- !1680 merged stable — Npcap 1.10 update including release notes and package hashes.
- !1679 merged — MP-QUIC draft support. Alexis requested native bitmask API and a pcap; Ivan Nardi requested exact draft/version documentation and splitting an unrelated correctness fix because it was backport material while the feature might not be. Backport-scope lesson promoted.
- !1678 merged — PER follow-up removes obsolete expert state and fixes wording.
- !1677 merged — pluginifdemo CMake warning fix.
- !1676 merged — LwM2M spec update.
- !1675 merged — NAS 5GS standardized SST value strings.
- !1674 merged — John Thacker macOS Qt setup maintenance, merged by Guy Harris; narrow build tooling.
- !1673 merged stable — DoIP 2019 backport.
- !1672 merged stable — DoIP 2019 backport.
- !1671 merged stable — SIP multiple contact-param parsing backport.
- !1670 merged stable — SIP multiple contact-param parsing backport.
- !1669 merged — Anders Broman bounds PER Open Type child TVBs to captured bytes, emits expert info, and carries the bounded effective length downstream. Promoted to resilience guidance.
- !1668 merged — PDCP-NR comment cleanup.
- !1667 merged — RPC stats removes an always-zero table index.
- !1666 merged — user-guide spelling cleanup.
- !1665 merged — DHCP stats removes an always-zero table index.
- !1664 merged — DVB-CI prototype parameter names updated from stale circuit terminology to conversation terminology.
- !1663 closed — proposed dumpcap ring-buffer EOF change; Anders Broman invited rebase/reopen if still relevant. No accepted implementation precedent.
- !1662 merged — primary DoIP 2019 support; discussion requests a shareable capture, corroborating sample-capture practice.
- !1661 merged — TCP conversation reuse fix with extensive John Thacker review: perform side-effect-free lookup while choosing identity, explicitly advance `last_frame` once chosen, and consider both exact directions. Promoted.
- !1660 merged — MaxMind synchronous mode preserves one response per request, including not-found results, avoiding blocking deadlock; shared response processing and public symbol bookkeeping are included. Promoted.

## Durable notebook updates

This run updates `text-encoding-conventions.md`, `field-display-policy-conventions.md`, `heuristic-dissector-conventions.md`, `conversation-api-conventions.md`, `subprocess-ipc-lifecycle-conventions.md`, `dissector-resilience-conventions.md`, `submission-backport-scope-conventions.md`, and `submission-conventions.md`, and adds `tap-listener-lifecycle-conventions.md`.

No SMPTE ST 291/VANC packet type was encountered.
