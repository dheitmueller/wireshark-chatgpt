# Wireshark MR review automation: !18509-!18558

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: newest to older. This run reconstructed the already-reviewed set from `reviewed-mrs-automation/` plus `reviewed-mrs.md`, preserving and counting the historical !17571-!17620 batch, then selected the fifty highest-numbered corpus MRs not already reviewed. The exact set for this run is the contiguous !18509 through !18558 range (50 MRs).

## Exact reviewed set

!18558, !18557, !18556, !18555, !18554, !18553, !18552, !18551, !18550, !18549,
!18548, !18547, !18546, !18545, !18544, !18543, !18542, !18541, !18540, !18539,
!18538, !18537, !18536, !18535, !18534, !18533, !18532, !18531, !18530, !18529,
!18528, !18527, !18526, !18525, !18524, !18523, !18522, !18521, !18520, !18519,
!18518, !18517, !18516, !18515, !18514, !18513, !18512, !18511, !18510, !18509.

## Weighting and durable findings

- **!18558 (merged; Guy Harris):** comments in `epan/packet.c` were changed to refer to dissectors by their registered names ("frame" and "file") rather than internal function names, because implementation function names are subject to change and one comment was already inaccurate. Useful high-authority documentation/API-boundary corroboration: comments should describe stable interfaces/semantic identities rather than incidental implementation names.
- **!18555 (merged; Martin Mathieson):** O-RAN FH CUS changes `udCompHdr` detection from one global boolean assumption to an optional heuristic whose result is stored per flow, because a single capture can contain flows requiring different interpretations. This reinforces existing state-identity guidance: inferred protocol state belongs at the narrowest semantic scope for which the inference is valid.
- **!18531 (merged; Anders Broman merge):** TCP sequence analysis implements RFC 6675 SACK-aware fast-retransmission classification. The submission supplied focused captures identifying exact packets whose classification changes. This is strong testing corroboration for stateful-analysis changes: provide captures with packet-level expected outcomes, especially where classification depends on preceding traffic history.
- **!18527 (merged; John Thacker):** JSON absolute-time output is forced to ISO 8601/UTC independently of presentation-oriented time preferences, while human-facing outputs retain compatibility controls. This is direct earlier corroboration of the later `locale-serialization-conventions.md` rule already established from !19316/!19329: machine-readable serialization has its own stable representation contract and should not inherit human display formatting accidentally.
- **!18526 (merged):** reverted !18514's EK absolute-time change before the more carefully scoped !18527 solution. Weight the final !18527 design over the reverted intermediate behavior.
- **!18523 (closed):** first Synphasor compliance submission closed unmerged; !18524 is the successor and carries the accepted form. Treat !18523 only as superseded context.
- **!18537 (merged):** IPFIX minimum-length calculation accounts for variable-length fields rather than treating a template containing only variable fields as having no useful minimum. Corroborates existing length/accounting guidance.
- **!18532/!18533 and !18525:** Strawberry Perl/CMake dependency-path handling evolved across multiple submissions; accepted later changes and WSDG documentation emphasize avoiding unrelated toolchain prefixes whose bundled libraries/tools can contaminate Windows builds. This is build-environment-specific corroboration rather than a new general convention.
- The ASN.1 dissector upgrades (!18543, !18544, !18547, !18548, !18549), field-deduplication series (!18509-!18512 and related), automatic updates (!18540-!18542), small documentation/build fixes, and backports were scanned and did not add durable conventions beyond rules already present in the notebook.

No convention file was modified in this run: the strongest findings are independent corroboration of existing notebook rules, particularly semantic state scoping, machine-readable serialization, focused capture-based testing, and preferring stable semantic interfaces over implementation details.

## Continuation

Continue backward from the highest-numbered corpus MR not present in any review ledger. With the corpus snapshot used here and the ledgers present at selection time, the next expected frontier is below !18509, but future runs must reconstruct the reviewed set rather than assuming a contiguous numeric range.
