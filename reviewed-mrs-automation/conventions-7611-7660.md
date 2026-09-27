# Conventions extracted from !7611-!7660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly !7660 down through !7611. Merged work is primary evidence; open/closed MRs are explicitly marked as provisional or process-only evidence.

## Durable rules promoted in this run

- **Preference cardinality must match Decode As cardinality.** !7611 and !7637 move automatic port preferences to ranges because Decode As can bind multiple values; a scalar cannot round-trip the real state.
- **Preference migrations must preserve persisted names.** !7658 maps differently named legacy preferences into the automatic table preference rather than discarding existing profile configuration.
- **Validate serialized limits at two layers.** Guy Harris's !7614 rejects overlong pcapng comments where users enter them, while explicitly calling for the core wiretap layer to enforce the same writer invariant for non-UI callers.
- **Tap/follow output should correspond to the promised semantic layer.** John Thacker's !7624 moves HTTP/2 Follow Stream header output after HPACK decompression and updates tests to assert decoded rather than compressed bytes.
- **Understand wmem scopes before acting on static-analysis leak reports.** Pascal Quantin's !7626 explains that `pinfo->pool` owns the returned storage; Coverity's report is a false positive because it does not understand packet-scope lifetime.
- **Unknown state must be distinct from a valid zero value.** !7660 changes algorithm state so protocol value 0 is not confused with "not known yet."
- **Strengthen heuristics with independent consistency checks.** John Thacker's !7648 layers ICV-size, padding, and subdissector-acceptance checks for ESP NULL recognition.
- **Do not overwrite protocol phase state before the next message consumes it.** MySQL !7621 and !7612 show how TLS/auth transitions fail when request state is replaced too early.
- **Representative files matter for wiretap variants too.** In !7639 Guy Harris asked for actual files using the new BLF header formats; the contributor supplied one for each format.
- **Keep generated build outputs and local-environment artifacts out of review.** Closed !7659 has strong Guy Harris/Pascal Quantin submission feedback even though its implementation is not accepted precedent.
- **Use a topic branch that permits maintainer collaboration.** Closed !7623 was replaced by merged !7629 after the contributor moved off protected `master` so the maintainer-commit workflow could be enabled.

## Provisional architecture evidence

Open !7638 proposes a display-filter result cache. Roland Knall identified several invalidation sources and Guy Harris summarized the dependency boundary: anything that forces packet redissection must invalidate the cache. Because the MR remains open in this corpus snapshot, retain this as provisional architecture guidance rather than accepted implementation precedent.

Open !7625 contains no comparable maintainer design conclusion and is retained only as a reviewed snapshot.
