# Wireshark MR review findings — !10312–!10300 plus !9999–!9963

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs. The exact selection is !10312 through !10300 followed by !9999 through !9963, because the intervening !10299 through !10000 MRs already have earlier per-run ledgers. Selection was checked against the available per-run ledger inventory, `reviewed-mrs.md`, and aggregate tracking rather than inferred from a range. The historical !17571–!17620 ledger was re-opened and confirmed to contain 50 unique entries.

Outcome weighting: 48 MRs were merged. !9977 and !9976 were closed and down-weighted. The corpus file for !9976 is empty; upstream GitLab was used only to recover its identity/state so the exact reviewed set could be correct.

## Strong durable findings

### !9963 / !9964 / !9965 — failure-only diagnostics are invalid on success

Guy Harris's master fix !9963 corrects sharkd after `dfilter_compile()` succeeds but the filter contains deprecated tokens. The old path tried to use the compile-error result as warning text even though there was no compile failure. The accepted path reports the deprecation independently. Guy also authored the release-4.0 and release-3.6 backports !9964 and !9965. Promoted to `error-path-contract-conventions.md` as the converse of the existing rule against using success-only outputs after failure.

### !9999 — guard state-machine transitions before mutation

Guy Harris reworks `json_dumper` so stack overflow and underflow are detected before push/pop mutations, and one error routine records the failure state and useful transition context. Once those boundaries are enforced, stack depth can use its natural unsigned domain. Promoted to `api-precondition-validation-conventions.md`.

### !10301 — bound child tvbuffs to semantic structures

Guy Harris changes NHRP so the Mandatory Part helper receives a tvbuff limited to the Mandatory Part rather than one that also contains the following extensions. The parent that knows the boundary enforces it before delegation. Promoted to `framing-boundary-conventions.md`.

### !9996 — reassembly identity is compositional

SOME/IP-TP needed more than an address/port key. During review Lars Völker also caught that replacing it with protocol identifiers alone would create a different collision because distinct service instances can be separated by endpoint information. The merged structured key combines the transport tuple with SOME/IP service/method/client/session/message identity. Promoted to `stateful-reassembly-conventions.md`.

### !9991 — heuristic TRUE is an ownership claim

The SOME/IP UDP heuristic could report success even after its protocol test rejected the packet, causing unrelated traffic to be claimed. The merged fix returns FALSE on failed recognition. Promoted to `heuristic-dissector-conventions.md`, including a recommendation to test negative traffic as well as positive captures.

### !9977 — stable branches do not take new features

!9977 attempted to carry already-merged IPv6 APN6 feature support to release-4.0. Jaap Keuter cited the Wireshark release policy and closed it because new features are not backported to stable releases. The implementation is down-weighted because it was not merged, but the maintainer policy statement was added as corroboration to `submission-backport-scope-conventions.md`.

## Additional evidence

- !10305, authored and merged by John Thacker, uses the nested-dispatch API variant that avoids repeatedly adding DRDA to the frame protocol list.
- !10312, also by John Thacker, reuses one DRDA SQL statement dissector handle for SQLSTT and SQLATTR because both contain the same FD:OCA object.
- !10309 and backport !10306 correct BFCP attribute length by accounting for the two-byte type/length header exactly.
- !10302 and stable backports !10303/!10304 correct a two-bit ISAKMP field's encoding semantics.
- !9983 contains a useful John Thacker/Lars Völker discussion of the historical `FT_DOUBLE` unit-rendering API limitation; it was retained as historical context rather than promoted as a timeless rule.
- !9982 avoids calling USBLL transfer-completion logic unless an active transfer key exists, corroborating the existing helper-precondition guidance.
- !9979 supplied a Wi-SUN EAPOL-Key pcap and further corroborates the representative-capture practice.
- !9968 and follow-up !9988, both authored by Guy Harris, make sharkd JSON marshalling follow JSON object/array structure; !9988 importantly corrects the assumption that every nested value needs a member name because array elements do not.
- !9995 shows a useful source-of-truth review habit: an apparently missing Netlink numeric value was checked against Linux rather than filled in by assumption.
- !9976 was counted but intentionally given essentially no architectural weight. Its corpus JSON is empty and upstream shows a closed draft with a large unrelated diff footprint and no substantive review.

## Next frontier

MR !9962 exists at the same corpus commit, is merged, and was inspected only for metadata as the next descending frontier. It was not reviewed or counted.
