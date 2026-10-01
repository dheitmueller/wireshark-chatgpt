# Review Findings — Wireshark !3161–!3210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

All 50 MRs in the exact ledger were inspected for state, discussion, and diff content. Merged master MRs were weighted most heavily. Closed !3171 and !3162 were down-weighted; !3162 is superseded by merged !3163.

## Highest-value durable evidence

- **!3209 / !3207:** repeated packet-controlled parsing must advance or terminate. Once an outer validator proves that only recognized enum values reach a helper, an unhandled default is an internal invariant and can be asserted.
- **!3206:** Bluetooth Mesh reassembly identity needs security/IV context in addition to source and short sequence identity.
- **!3205:** Kerberos ASN.1 work changed authoritative ASN.1/config/template inputs and regenerated C together, with a capture and key material supplied for review.
- **!3202:** display-relative SCTP TSNs must not replace raw sequence values inside analysis state. Raw companion fields preserve the stable protocol value.
- **!3201:** implementation-only Wiretap helpers belong on the internal API surface; public headers and package symbols are compatibility promises.
- **!3183 / !3185:** malformed-file diagnostics should identify the format/subsystem and violated condition rather than internal helper-function names.
- **!3188:** amending a commit message does not update the GitLab MR description; the two review artifacts must be kept synchronized deliberately.
- **!3171:** closed/superseded Thrift work provides negative evidence that written protocol documentation can itself be wrong and samples must cover semantically distinct encodings.
- **!3166:** build-time and runtime dependency versions are separate diagnostic facts and can differ.
- **!3164:** Exported PDU direction metadata preserves point-to-point direction across export and redissection.

!3200 and !3199 corroborate the master Ascend fix in !3198. !3194 supersedes the mechanism introduced by !3181. !3162 is a closed predecessor of merged !3163.
