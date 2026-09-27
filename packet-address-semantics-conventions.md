# Wireshark Packet Address Semantics Conventions

This file records durable conventions for packet address state, columns, and filtering extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Populate semantic address state instead of writing presentation columns directly

When Wireshark already has semantic packet-address state from which columns and filters are derived, dissectors should populate that state instead of writing only the displayed column text. Presentation-only writes can be overwritten by later generic column processing and leave filtering/navigation behavior disconnected from what the user sees.

Merged master MR !14689, authored by John Thacker and approved/merged by Anders Broman, fixes IEEE 802.11 RA/TA and AID columns. The old code wrote `COL_RES_DL_SRC` / `COL_RES_DL_DST` text directly. That only survived when `pinfo->dl_src` / `pinfo->dl_dst` were otherwise unset; when WLAN was tunneled (for example Aruba ERM over UDP), later generic address-column generation could overwrite the manually written text with an outer address. Direct text also did not provide the semantic address/filter relationship needed for right-click filter generation. The accepted implementation registers address types for RA/TA and AID, stores them in `pinfo->dl_src` / `pinfo->dl_dst` (and the corresponding source/destination addresses), and supplies address-type filter callbacks so generic framework behavior remains coherent.

**Architecture rule:** if framework presentation is derived from semantic packet state, update the semantic state and let the framework render it. Do not treat a column string as the authoritative data model when an address/value object exists that also drives filtering, resolution, and late column regeneration.

**Review rule:** test address-column changes in encapsulated/tunneled captures as well as top-level link-layer captures, and verify generated filters in addition to displayed text. A fix that only changes `col_*()` output may appear correct in the simple case while remaining semantically wrong underneath another encapsulation.

**Confidence:** Very high. Merged master architecture fix authored by John Thacker and approved/merged by Anders Broman, with the tunneling overwrite and filter-generation failure explicitly documented in the MR. MR !14694 immediately follows with an AID string-size/filtering correction and independently reinforces that custom address presentation must honor the address API's formatting contract.


## Generic numeric addresses use host-order semantic values

Merged !7228 made `AT_NUMERIC` width-aware, but review exposed an endian contract problem. Pascal Quantin flagged integer-format portability; Brian Sipos and João Valverde argued that the generic address type should not impose little-endian storage. Merged follow-up !7235 converts openSAFETY values at the protocol boundary and defines `AT_NUMERIC` values in host byte order.

**API rule:** convert protocol/network byte order before constructing a generic numeric address. Keep the generic address representation host-semantic rather than encoding a wire-endian policy into the address API.

**Width rule:** preserve the full numeric width through sorting and formatting. Review of !7228 also caught a Qt `QString::toInt()` path that could not represent the full 64-bit domain.

**Confidence:** Very high. Two merged master changes with direct Pascal Quantin, João Valverde, Brian Sipos, and Roland Knall review.
