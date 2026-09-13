# Automated Wireshark MR review: !20739-!20788

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection rule: rebuilt the already-reviewed set from all available files in `reviewed-mrs-automation/` plus sparse entries in `reviewed-mrs.md`, preserving the separately reviewed !17571-!17620 batch. Selected the fifty highest-numbered corpus MRs not already present in that tracking. The resulting set is exactly !20788 through !20739 inclusive; all 50 IDs are present in this corpus commit.

Weighting rule: merged master MRs receive the most weight; stable backports mainly corroborate their master changes; closed/draft/superseded MRs are retained in the ledger but are not treated as accepted architectural precedent. Senior-maintainer-authored changes and substantive maintainer review are weighted especially strongly.

## Exact reviewed set

| MR | Review level | Notes |
|---|---|---|
| !20788 | Scanned | SLL2 reserved bytes represented explicitly in the protocol tree; merged. |
| !20787 | Deep | SOME/IP TCP recovery validates plausible headers before trusting length/framing after a missing segment; merged. Useful robustness corroboration. |
| !20786 | Scanned | Qt Windows hover-color preference handling; merged. |
| !20785 | Scanned | wmem_map deleted-item bookkeeping switched to stack semantics; merged. |
| !20784 | Scanned | O-RAN SINR display refinement; merged. |
| !20783 | Deep | John Thacker-authored/merged BACapp crash fix: custom formatters may run when ambient packet scope is not active. Promoted to `allocator-scope-conventions.md`. |
| !20782 | Scanned | Qt packet-list/profile-change sequencing follow-up. |
| !20781 | Scanned | Earlier Qt packet-list/profile-change iteration; lower weight than successor !20782. |
| !20780 | Deep | Michael Mann RPC cleanup threads `packet_info *` through helpers, replaces ambient `wmem_packet_scope()` with `pinfo->pool`, and removes NULL packet-info helper calls. Promoted as corroborating evidence in `allocator-scope-conventions.md`. |
| !20779 | Scanned | WSDG wiretap documentation chapter; merged. |
| !20778 | Scanned | BACapp friendlier object decoding / custom formatting; merged. |
| !20777 | Scanned | ILNP restricted to IPv6 context; merged. |
| !20776 | Scanned | Stable backport of SAPB NULL-tvb length fix; corroborating only. |
| !20775 | Scanned | Stable backport of SAPB NULL-tvb length fix; corroborating only. |
| !20774 | Scanned | BIST TotalView-ITCH dissector addition; merged successor to earlier drafts. |
| !20773 | Scanned | BLF release-note update; merged. |
| !20772 | Deep | Large integer/picosecond calculation gets compiler-portability sanity checking where 128-bit arithmetic is unavailable; merged. Corroborates arithmetic portability guidance. |
| !20771 | Scanned | Master SAPB NULL-tvb length fix; merged. |
| !20770 | Deep | SOME/IP bitfield support plus datatype/profile sanity checks; merged. Corroborates defensive schema/profile validation. |
| !20769 | Scanned (closed/draft) | Earlier BIST-ITCH draft; superseded by later merged work and not treated as precedent. |
| !20768 | Scanned | collectd expert-info call corrected to pass the proper tree type; merged. |
| !20767 | Deep | TCP profile switching crash fix after preference-dependent analysis fields; merged. Useful preference/lifecycle correctness example. |
| !20766 | Scanned | Dissector-warning cleanup; merged. |
| !20765 | Scanned | WSDG file-set documentation update; merged. |
| !20764 | Scanned | ILNP TCP MSS support; merged. |
| !20763 | Scanned | XML uses wmem-native lowercase helper instead of GLib allocate/copy/free chain; merged. |
| !20762 | Scanned | Moves expensive filter-name validation into dedicated assertion/check path; merged. |
| !20761 | Scanned (closed/draft) | Initial BIST-ITCH draft; superseded by later work and down-weighted. |
| !20760 | Scanned | Stable/backport form of ciscodump cleanup fix; corroborating only. |
| !20759 | Scanned | BLF seek-condition bug fix; merged. |
| !20758 | Scanned | GSM SIM STORE DATA / SGP.32 dispatch addition; merged. |
| !20757 | Scanned | UAT help links redirected to generic UAT help; merged. |
| !20756 | Scanned | Removes broken IKEv1 UAT help link; merged. |
| !20755 | Deep | XML flat namespace avoids unnecessary element copying; merged. Corroborates ownership/copy-semantics review. |
| !20754 | Scanned | ICMP echo payload gets explicit tree item while retaining timestamp interpretation; merged. |
| !20753 | Scanned | Automatic generated-data update; merged. |
| !20752 | Scanned | Automatic generated-data update variant where manuf failed; merged. |
| !20751 | Scanned | Automatic generated-data update variant where manuf failed; merged. |
| !20750 | Scanned | Roon dissector update; merged. |
| !20749 | Deep | Radius dictionary functionality moved out of generic epan into the dissector layer because only the RADIUS dissector uses it; merged. Strong corroboration of source-layering guidance already recorded from later MRs. |
| !20748 | Scanned | Npcap reverted to 1.80 as workaround for regressions in newer versions; merged. |
| !20747 | Scanned | Stable backport of nstime negative-subsecond fix; corroborating only. |
| !20746 | Deep | HTTP/3 datagram dissection adds protocol-specific dispatch table and RFC 9297 handling; merged. Useful dispatch-architecture exemplar but no separate notebook rule needed. |
| !20745 | Scanned | BER file opening prefers Decode As syntax; merged. |
| !20744 | Scanned | Qt preference/UAT help URLs with fragments fixed on Windows; merged. |
| !20743 | Deep | Master nstime negative-subsecond correctness fix; merged. Corroborates signed time-domain handling. |
| !20742 | Deep | ZMTP preference changes invoke apply-prefs behavior rather than handoff registration again; merged. Reinforces registration-vs-runtime-configuration lifecycle separation. |
| !20741 | Scanned | Optimizes duplicate Decode As preference checks; merged. |
| !20740 | Scanned | Ixia/Keysight NetFlow fields updated; merged successor. |
| !20739 | Scanned (closed/superseded) | Earlier Ixia/Keysight NetFlow update; closed and superseded by !20740, so not treated as accepted precedent. |

## Durable notebook extraction

The principal promoted lesson from this batch is the packet-scope/callback-lifetime rule from !20783, strongly corroborated by !20780. `allocator-scope-conventions.md` now records that deferred/custom formatting and printing paths cannot assume ambient `wmem_packet_scope()` is active; when packet context exists, allocation should use explicit `pinfo->pool`, and APIs without packet context should avoid packet-scope allocation or change their contract.

Other MRs chiefly corroborated conventions already present in the notebook rather than justifying duplicate rules: !20749 reinforces source-layering boundaries, !20772 reinforces portable arithmetic limits, !20755 reinforces ownership/copy semantics, and !20742 reinforces separating registration/handoff from preference-application lifecycle.
