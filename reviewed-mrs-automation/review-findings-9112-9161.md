# Wireshark MR Review Findings: 9112-9161

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

This batch contains 45 merged MRs, three closed/unmerged MRs (9160, 9140, 9127), and two open/unmerged MRs (9131, 9117). Merged master changes carry the most weight. Stable-branch backports primarily corroborate their master changes, while unmerged or superseded work is treated as review/process evidence only.

## Durable findings

### Logical session identity can be narrower than a transport 5-tuple

Merged master MR 9141, authored by John Thacker and merged by Gerald Combs, fixes QUIC when multiple logical connections share one UDP 5-tuple. The accepted design keeps transport-level state for locating candidate QUIC connections but gives each QUIC connection a protocol-specific identity keyed by Wireshark's QUIC connection number. TLS and other subdissectors therefore attach state to the logical QUIC connection rather than accidentally sharing it across every connection on the same addresses and ports. Review included the issue capture, a separate connection-migration capture, and Follow Stream checks.

MR 9123 is corrective evidence: John Thacker later explained that lower-layer UDP/TCP dispatch consults the transport conversation, whereas a protocol-specific conversation is appropriate for protocol state. Those are distinct responsibilities.

### Packet-derived offset arithmetic must be overflow-safe

Merged master MR 9124, authored by Gerald Combs, checks offset addition before advancing by packet-controlled lengths. Overflow produces malformed-input diagnostics and a bounds exception instead of unsigned wraparound. Release-4.0 MR 9134 and release-3.6 MR 9135 carry the same correction. The master change also makes unknown-type recovery advance rather than feeding a negative length/alignment into later arithmetic.

### Retained state must own packet-derived strings

Merged master MR 9114, authored and merged by João Valverde, fixes a lifetime bug in SIP resend tracking. A long-lived transaction record retained a parsed method string whose storage did not live as long as the record. The accepted fix duplicates that string into file scope before storing it.

### Bound presentation caches and avoid unrelated cache churn

Merged master MR 9143 changes packet-list column strings to a bounded cache and prevents colorization-only passes from filling and evicting column data they do not need. Discussion recognizes that sorting may require the full working set and should have deliberate resource behavior rather than pathological repeated cache misses.

### Copy/export should follow the visible view

Merged master MR 9120 changes TrafficDialog CSV/YAML/JSON copying to iterate the filtered/proxy model instead of the unfiltered source model, so copied output matches what the user can actually see.

### Build human-readable UTF-8 dynamically

Merged master MR 9132, authored and merged by João Valverde, removes fixed-size display buffers and uses dynamically sized formatting/string-buffer APIs. Arbitrary byte caps could truncate UTF-8 in the middle of a multibyte code point. MR 9153, authored by John Thacker with extensive Guy Harris review, independently reinforces careful charset progress accounting and compiler-portability reasoning.

### Validate operator capability during display-filter semantic checking

Merged master MR 9161 fixes a display-filter crash for unary minus on types that do not support negation. The semantic checker now rejects the unsupported operator/type combination before evaluation.

### Zero-length values still require structural state transitions

Merged master MR 9126, authored by John Thacker, fixes Protobuf-to-JSON output for zero-length byte strings. Once a field has been emitted, the base64 element must execute its begin/end state transitions even when there are zero payload bytes. Release-4.0 MR 9130 carries the same correction.

## Corroboration of existing notebook rules

MR 9145 is the master change behind later TCP stable backports 9415 and 9416: foundational stream-identity state cannot be gated behind optional Sequence Number Analysis.

MR 9149 is early accepted architecture for persistent/transitive frame dependencies. Later reviewed fixes add the stronger reset, duplicate-suppression, and hash/set scaling rules; the early list representation is historical rather than preferred.

MR 9158 shows the typed-item checker catching two real WLAN registration mistakes: a duplicate filter abbreviation and an unintended mask.

MR 9137 makes Ethernet-address custom columns use normal resolved/raw address presentation. MR 9115 plus stable backport 9128 updates both the NBAP ASN.1 template and generated output while adding UE context needed for RLC reassembly. MR 9113 replaces a protocol-private APN decoder with the shared text-decoding helper.

## Non-merged review evidence

Closed MR 9140 contains high-authority design feedback from Guy Harris, supported by Anders Broman, questioning the addition of a preference when the information could simply be shown. It was superseded, so this is design guidance rather than accepted implementation evidence.

Open MR 9117 contains Stig Bjørlykke's caution that applying another platform's filename restrictions globally can break existing profiles. Open draft MR 9131 has no substantive maintainer acceptance. Closed MRs 9160 and 9127 were superseded/already fixed. These unmerged changes were down-weighted.
