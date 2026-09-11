# Wireshark MR Review Automation: !24059–!24108

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Selection was made from the exact union of entries in `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run ledgers under `reviewed-mrs-automation/`. Numeric filename ranges were not treated as proof that every MR in a range was reviewed. The historical !17571–!17620 batch remains preserved and counted.

Exactly 50 previously unreviewed corpus MRs were reviewed in descending order: !24108 through !24059 inclusive.

| MR | Review status | Notes |
|---|---|---|
| !24108 | Scanned | Merged release-4.6 backport of the IEEE 802.11 EHT field-width correction; no new convention beyond correct hf width/type matching. |
| !24107 | Discussion-focused (open) | Early disabled-protocol loading optimization. Stig Bjørlykke demonstrated that the proposed state handling breaks profile switching; Jaap Keuter also requested motivation in the commit message. Down-weighted because unresolved and unmerged. |
| !24106 | Scanned | Merged release-4.4 backport of the Zigbee Direct pre-decryption bounds hardening represented by !24104. |
| !24105 | Scanned | Merged release-4.6 backport of !24104. |
| !24104 | Deep | Merged John Thacker Zigbee Direct hardening validates encrypted length before key trials/decryption, avoids unnecessary copying, and uses a value length contract rather than mutable length state. Strongly corroborates existing hostile-length and state-contract guidance. |
| !24103 | Scanned | Merged release-4.4 backport of the BEEP overflow/recursion fix represented by !24101. |
| !24102 | Scanned | Merged stable backport of the BEEP overflow/recursion fix represented by !24101. |
| !24101 | Deep | Merged John Thacker BEEP fix rejects illegal packet-controlled sizes with normal bounds failure instead of trying to massage overflowed values into loop progress; also corrects unsigned value domains. Corroborates existing malformed-input and progress rules. |
| !24100 | Scanned | Merged USB-HID byte-order and field-width correction with test updates; protocol-specific correctness work, no new cross-cutting convention. |
| !24099 | Scanned | Merged O-RAN Coverity resource-lifetime cleanup; reinforces existing static-analysis/resource-ownership guidance. |
| !24098 | Scanned | Automatic data/translation update; no reusable engineering lesson. |
| !24097 | Scanned | Automatic data/translation update; no reusable engineering lesson. |
| !24096 | Scanned | Automatic data/translation update; no reusable engineering lesson. |
| !24095 | Scanned | Release-note update for the profile-import Zip-Slip/security fix; documentation-only. |
| !24094 | Scanned | Release-note update for the profile-import Zip-Slip/security fix; documentation-only. |
| !24093 | Scanned | Release-note/security notice for the profile-import RCE fix; documentation-only. |
| !24092 | Scanned | MKA Coverity nullability cleanup; legitimate analyzer finding but no new rule beyond existing static-analysis guidance. |
| !24091 | Scanned | QCDIAG cleanup plus Coverity fixes; useful correctness maintenance without distinct reusable review guidance. |
| !24090 | Scanned | Stable backport of WSLua integer `tostring` format corrections. |
| !24089 | Scanned | Stable backport of WSLua integer `tostring` format corrections. |
| !24088 | Scanned | Merged dissector double-fetch cleanup; corroborates the established fetch-once/ret-value-helper idiom. |
| !24087 | Discussion-focused (open) | 802.11 Decryption Secrets Block proposal. Guy Harris explicitly required registering the new on-disk DSB secret type through the pcapng specification first. High-authority format-governance evidence, but retained as provisional because the MR remains open/unmerged. |
| !24086 | Scanned | Merged master WSLua integer formatting correction; narrow API-format correctness cleanup. |
| !24085 | Scanned | Merged MKA/MACsec XPN high-32-bit recovery follow-up; protocol-specific extension of !24060. |
| !24084 | Deep | Merged MKA/MACsec refinements remove duplicate lookup and improve SCI/payload diagnostics; primarily corroborates existing fetch-once and diagnostic guidance. |
| !24083 | Scanned | Spelling-only cleanup. |
| !24082 | Scanned | Merged ETSI DCP cleanup removes double-fetches and aligns field types/helpers with semantic values; corroborates existing field/helper rules. |
| !24081 | Scanned | Stable backport of the ETSI DCP Reed-Solomon heap-overflow fix represented by !24078. |
| !24080 | Scanned | Release-4.6 backport of !24078. |
| !24079 | Scanned | CMake installation option for global profile data directory; build/configuration feature with no distinct review convention. |
| !24078 | Deep | Merged John Thacker ETSI DCP heap-overflow fix allocates the extra capacity required by in-place Reed-Solomon decoding and rejects nonsensical packet-controlled parameters before arithmetic/decoding. Corroborates existing capacity and precondition guidance. |
| !24077 | Deep | Merged NMF PDU-length hardening uses a zero-based subset TVB to prevent `offset + length` overflow. Strong corroboration of the existing subset-TVBuff arithmetic rule. |
| !24076 | Scanned | VXLAN tree-summary enhancement exposing VNI/Group Policy ID; presentation-only. |
| !24075 | Scanned | Qt accessibility names/descriptions for hierarchy; UI accessibility improvement, no new cross-cutting rule. |
| !24074 | Scanned | ENIP TCP/IP Interface Object attribute additions; straightforward protocol coverage. |
| !24073 | Scanned | MKA duplicate cipher-suite definition cleanup; implementation hygiene only. |
| !24072 | Deep | Merged RDP reassembly hardening handles captures missing the first fragment instead of dereferencing absent chunk state. Corroborates existing reassembly partial-capture/state-initialization guidance. |
| !24071 | Scanned | O-RAN tap column construction converted to `wmem_strbuf`; resource/string-building cleanup without new convention. |
| !24070 | Scanned | Stable backport of SSH MAC-length bounds hardening represented by !24066. |
| !24069 | Scanned | Stable backport of SSH MAC-length bounds hardening represented by !24066. |
| !24068 | Scanned | EtherCAT Mailbox-VoE dissector addition; straightforward protocol extension. |
| !24067 | Scanned | ENIP QoS attribute additions; straightforward protocol extension. |
| !24066 | Deep | Merged SSH hardening avoids comparing a packet-declared MAC length larger than the implementation's supported/allocated MAC buffer. Corroborates capacity-before-compare/copy guidance. |
| !24065 | Deep | Merged Coverity untrusted-loop-bound cleanup. Review distinguishes bounded small-width domains from analyzer-significant tainted loop bounds and uses explicit masking where semantically valid; corroborates existing static-analysis guidance rather than creating analyzer-driven semantics. |
| !24064 | Deep | Merged TCP first-reset attribution fields. Jaap Keuter explicitly corrected expert severity: the reset itself is warning-worthy, while derived endpoint-attribution information is NOTE/CHAT level. Promoted as independent corroboration in `protocol-input-diagnostics-conventions.md`. |
| !24063 | Scanned | Diameter MWD-Status decoding addition; protocol-specific coverage. |
| !24062 | Scanned | Diameter S6c AVP additions/enumeration correction; protocol-specific coverage. |
| !24061 | Scanned | Qt accessibility exposure of packet flags and expert severity; UI accessibility improvement. |
| !24060 | Deep | Merged John Thacker MKA/MACsec XPN implementation creates persistent SAK state only on first pass and retrieves it later, supports rekeying where nominal identifiers repeat, and includes a focused sample capture. Strong corroboration of existing redissection/state-identity rules. |
| !24059 | Deep | Merged John Thacker BZR fix moves PDU-length parsing into a zero-based subset TVB and requires strict forward progress (`new > old`), preventing unsigned `offset + length` wrap and zero-progress malformed results. Strong corroboration of existing subset-TVBuff and monotonic-progress rules. |

## Durable notebook effect

- `protocol-input-diagnostics-conventions.md` was strengthened with !24064 as independent merged evidence that expert severity should track operational significance: the event can be warning-worthy while derived explanatory/attribution metadata remains NOTE/CHAT.
- !24087 records high-authority Guy Harris guidance that a new pcapng DSB secret-type identifier should first be standardized in the pcapng specification. Because the MR is still open, this was intentionally **not** promoted to a durable accepted-project rule yet.
- !24104, !24101, !24078, !24077, !24072, !24066, !24060, and !24059 mainly corroborate existing notebook rules on hostile length validation, capacity, subset TVBs, strict progress, partial-capture state, redissection, and state identity; duplicate rules were not added.
