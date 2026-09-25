# Wireshark MR review findings: !9312-!9361

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 previously unreviewed MRs were examined, newest to oldest. The companion ledger records the exact set. Merged changes were weighted more heavily than closed drafts; accepted successors were preferred when available.

## Durable findings

### !9359: Composite TVBuff lifecycle

HTTP/2 and WSCBOR both exposed the same lifecycle problem: a composite TVBuff should not be created speculatively when zero components might be produced. The accepted change creates the composite lazily on the first append, finalizes it only when it exists, and represents a legitimate empty WSCBOR string with an ordinary zero-length subset TVBuff. An empty indefinite-byte-string unit test was added.

### !9336: Unmasked FT_BOOLEAN means zero versus nonzero

Martin Mathieson's merged change explicitly reverses several conversions to `FT_BOOLEAN`: without a mask, any nonzero value is true. A protocol field that is really a numeric/categorical domain with assigned values 0 and 1 should remain an integer field with a `value_string`; Boolean is appropriate only when the protocol defines a truth predicate.

### !9327: Runtime inability to reassemble is not an assertion failure

Gerald Combs changed TCP's unknown-length PDU path from `DISSECTOR_ASSERT` to `FragmentBoundsError` when another segment is requested but reassembly cannot occur. The MR notes ordinary runtime causes such as checksum validation and disabled reassembly. Packet-, preference-, or caller-dependent states belong in normal error flow, not programmer assertions.

### !9322, !9339, !9340: Proto-data lifetime follows the stored object

GeoNetworking stored packet-owned TVBuff pointers in file-scope proto data. Master !9322, authored and merged by Gerald Combs, changes both add/get calls to `pinfo->pool`; !9339 and !9340 backport the fix. A long-lived container does not extend the lifetime of packet-owned values stored inside it.

### !9349: Sorting is a whole-capture traversal

John Thacker's merged change makes packet-list sorting participate in the capture-file read lock, common stop/progress state, and post-read retry lifecycle. This independently corroborates the later !9452 whole-capture traversal convention already recorded in `capture-traversal-conventions.md`.

### !9348: Nominal ASCII still needs text-decoding semantics

ZDP moves isolated bytes through `format_char()` and uses a proper ASCII extraction path. This corroborates the later !9489 rule already recorded in `text-encoding-conventions.md`.

### Review-scope evidence

Closed !9334 is down-weighted as implementation evidence, but Anders Broman explicitly said the PFCP refactor had become too large to review effectively and requested smaller MRs with bug fixes separated. The contributor agreed; later merged !9363 and !9368 are stronger accepted evidence. Closed !9330 similarly provides lower-weight evidence for logical separation of unrelated changes, representative captures, and test-suite validation.

Other useful corroboration: !9355 shows Alexis La Goutte requesting Clang Analyzer cleanup and a release-note entry for a new dissector, with Windows CI later exposing a type-width mismatch fixed in !9375. !9314 fixes a GSList node/data confusion; Guy Harris authored the release-4.0 and release-3.6 backports !9318 and !9320. !9316 contains Pascal Quantin's guidance that observed live-network nonconformance should receive a specific protocol diagnostic rather than be silently normalized.

## Disposition summary

Deep or discussion-focused MRs with reusable evidence: !9359, !9355, !9349, !9348, !9344, !9336, !9335, !9334, !9330, !9327, !9322, !9320, !9318, !9317, !9316, !9315, !9314. The remaining MRs were inspected and classified as scanned because they were straightforward protocol/data/tooling changes, automatic updates, stable backports duplicating already-understood behavior, or contained no additional durable reviewer guidance.

Closed/unmerged MRs in the exact batch: !9344, !9334, !9330, !9325, !9323, !9316. They were down-weighted.

Frontier probe only: !9311 exists, is closed/unmerged, and was not counted as reviewed. The corpus is not exhausted.
