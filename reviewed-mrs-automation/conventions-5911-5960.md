# Durable conventions from !5911–!5960

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Highest-confidence additions

1. **Establish ownership before fallible initialization continues.** Guy Harris's merged !5911 moves a newly allocated Wiretap private object and its close callback into the owning `wtap` before later open-time failures. Ordinary teardown must be able to see partially initialized owned state.

2. **Initialize advertised outputs before the first normal error return.** Gerald Combs's merged !5912 sets Kafka's optional display-string output to a valid sentinel at function entry so malformed-length paths cannot leak stale caller state.

3. **Do not weaken programmer-error helper contracts to hide bad callers.** In merged !5922, João Valverde rejects replacing an invalid-pointer sentinel with plausible empty output; Guy Harris argues that such programmer errors should remain visible as dissector bugs. The accepted code fixes callers and asserts the positive-length helper precondition.

4. **Do not change API semantics merely to silence a static analyzer.** In merged !5920, João Valverde explicitly favors an intentional unused-value marker over inventing a return value that callers must not consume. !5926 complements this: zero-initialization is a valid warning fix only after confirming zero/NULL is semantically valid on all affected paths.

5. **A dissector's `void *data` is a real typed contract.** Stig Bjørlykke catches an HTTP/2 context being forwarded to DTAP, which expects a different structure; merged !5944 passes NULL instead, with stable backports !5951/!5952.

6. **Use Git revision operators that match the graph relation.** John Thacker's merged !5954 uses `HEAD~N` for N generations back, not `HEAD^N`, which selects parent number N. !5943 independently demonstrates the failure in a multi-commit CI job.

7. **Keep explicit Decode As identity separate from heuristic uncertainty.** John Thacker's merged !5950 preserves manual RTCP/SRTCP selections and gives ambiguous heuristic dispatch a preference instead of pretending it can infer the protocol. !5948 similarly avoids a false malformed-length diagnosis when required SRTCP metadata is unavailable.

8. **Repair accidental stable ABI breaks with narrow compatibility machinery and truthful package metadata.** Gerald Combs's release-3.6 !5958 restores a removed symbol as a deprecated wrapper and records that it reappeared in 3.6.2. !5937 shows that moving a public implementation across libraries also requires updating pkg-config/link metadata for external consumers.

## Lower-weight workflow corroboration

Closed !5934 and !5935 are not implementation precedent, but their maintainer feedback reinforces the existing submission rule: contribute from a topic/feature branch that permits maintainer collaboration rather than from a protected personal `master`. Their merged successors (!5936 and !5937) carry the implementation weight.

## Evidence weighting

The strongest new evidence in this run is !5911 (Guy Harris), !5912 and !5958 (Gerald Combs), !5922 (direct João Valverde and Guy Harris review), !5944 (Stig Bjørlykke review accepted by Pascal Quantin), !5950/!5954 (John Thacker), and the merged stable backports that confirm intended behavior. Closed/superseded work was used only to explain workflow or rejected/superseded directions.
