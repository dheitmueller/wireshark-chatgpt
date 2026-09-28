# Durable Wireshark conventions from !6761-!6810

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

## Optional Wiretap record metadata needs an explicit presence contract and read-path parity

Merged !6792, authored by Guy Harris, adds a section number to `wtap_rec`. The numeric member is initialized to zero, but validity is controlled independently by `WTAP_HAS_SECTION_NUMBER`. The pcapng reader sets both the flag and value in normal sequential reads and in seek reads. Frame dissection exposes the field only when the presence flag is set, while converting the internal 0-based section index to the user-facing 1-based number.

A default-initialized metadata value is not proof that the source supplied that metadata. When adding record metadata, carry an explicit presence/validity indicator, populate it consistently across sequential and random-access read paths, and keep internal-vs-display numbering conversion explicit at the presentation boundary.

## Protocol-only tree traversals should prune non-protocol nodes before recursion

Merged !6776, authored by John Thacker, changes protocol hierarchy statistics so non-protocol tree nodes are skipped before recursive calls rather than entered and discarded later. It also replaces a display-name heuristic with `proto_registrar_is_protocol()`. The change reduces recursive depth in unusual tree layouts that can otherwise create many top-level non-protocol items.

When an analysis consumes protocol nodes rather than arbitrary presentation nodes, use the registry's semantic protocol predicate and filter before recursion. Do not infer node kind from whether a label or name happens to be empty or nonempty.

## Conversation proto-data helpers require a real conversation object

Merged release backports !6779 and !6780 add explicit NULL checks to `conversation_add_proto_data()`, `conversation_get_proto_data()`, and `conversation_delete_proto_data()`, report the condition through Wireshark's dissector-bug mechanism, and document that the conversation argument must not be NULL.

Absence of a conversation and absence of protocol data are different states. Callers that do not have a conversation should branch before invoking conversation proto-data APIs; the shared helpers should treat NULL conversation input as a caller bug rather than silently returning “no data.”

## Raising a supported dependency floor should collapse obsolete compatibility paths

The John Thacker dependency series in !6771, !6773-!6775, !6784, !6787, !6793, !6795, and !6797 raises Qt, libgcrypt, CMake, GLib, GnuTLS, and nghttp2 minimums based on the supported platform matrix. The changes then remove source fallbacks, capability macros, test skips, packaging workarounds, and policy branches that are impossible under the new minimums. Older distributions remain supported by the applicable LTS release rather than forcing current master to retain dead compatibility code.

A baseline change is not complete when only `find_package()` changes. Synchronize source conditionals, tests, packaging requirements, setup scripts, documentation, and feature probes with the new support floor. Delete unreachable compatibility code instead of leaving a misleading alternate implementation in place.

## Unsupported build targets should fail during configuration

Merged !6789, authored by Gerald Combs, turns the already-deprecated 32-bit Windows target into an explicit CMake configuration error. Once a target is outside the supported matrix, do not let configuration continue far enough to produce a partial or misleading build. Fail early with a diagnostic that points to the support decision.

## Evolving capture state belongs to the first pass; packet-specific derived state belongs with the packet

Merged !6763 updates CIP Safety timestamp/rollover state only while the frame is on its first dissection pass, stores the rollover value needed for CRC verification in per-packet proto data, and retrieves that stored value on redissection. It also encodes a protocol-specific synchronization invariant: rollover remains zero until the connection has observed timestamp state that makes rollover meaningful.

For state that is learned sequentially, mutate the running history only on the ordered first pass. If a packet's later interpretation depends on the state derived at that point in history, persist that value with the packet so redissection is deterministic.

## Optional protocol suffixes should be decoded by valid layout boundaries, not one legacy exact length

Merged !6778 changes Couchbase DCP Snapshot Marker handling from an exact 36-byte assumption to a 20-byte mandatory prefix, an optional 16-byte extension, and a further optional 8-byte timestamp. Invalid intermediate lengths still receive expert diagnostics.

When a protocol evolves by appending optional fields, validate the mandatory prefix first, then decode each optional layout only when its complete extent is present. Do not preserve an obsolete exact-total-length check that turns a valid newer message into opaque data.

## CI jobs should exclude unrelated components that consume the job's budget without contributing coverage

Merged !6810, authored by Gerald Combs, disables `mmdbresolve` in fuzz and No-options builds after it began driving fuzz jobs into timeout. A specialized CI configuration should build the components necessary for the property it is testing. If an unrelated helper dominates runtime or failure behavior, disable it in that job rather than sacrificing the intended fuzz or build-variant coverage.

## Submission identity is part of submission correctness

During merged !6778 Alexis La Goutte requires the contributor to verify the GitLab account and correct commit author metadata before merge. Repository submission checks apply to commit metadata as well as source content; repair the commit identity on the existing MR branch rather than treating the pipeline failure as unrelated infrastructure.

## Weighting notes

All 50 MRs are merged. !6792 has exceptional authority because it is authored by Guy Harris. !6776 and the dependency-baseline series are authored by John Thacker. !6789 and !6810 are authored by Gerald Combs. The conversation NULL-precondition evidence in !6779/!6780 is carried here as two accepted stable-branch backports of the same master fix. Protocol-specific and packaging-only changes are retained in the exact findings ledger but are not promoted unless they materially strengthen a cross-cutting rule.
