# Durable conventions from Wireshark MRs !5611-!5660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Parser progress and malformed variable-length integers

Merged master !5626 and stable !5629 show that when `tvb_get_varint()` reports failure with an encoded length of zero, returning the unchanged input offset is unsafe if callers use that return value to drive an enclosing loop. The accepted behavior reports expert information and terminates the current parse by returning the capture end. Guy Harris's merged release-3.4 backport !5657 is especially strong corroboration. A valid encoded value that is semantically out of range is different: if bytes were consumed, the returned cursor should reflect that progress.

Merged !5660 is important negative evidence. It correctly recognized the need for a finite RTMPT AMF work budget and for propagating a zero/no-progress result to callers, but the budget predicate was written backwards. John Thacker later pointed directly to the error; merged !13933 is the authoritative correction. Guard conditions need tests for normal multi-iteration success as well as exhaustion.

## Output parameters remain part of the contract on error exits

Guy Harris's !5655 changes Kafka helpers so their returned offsets are values callers can actually consume and their optional offset/length outputs are initialized. Parser-progress hardening then removed some of those assignments, and merged master follow-up !5643 (with stable !5644 and !5658) restored them. A caller-visible output contract does not disappear just because the helper is returning from a malformed-input path.

## Resource limits should have an external/protocol rationale where possible

Guy Harris's !5656 replaces Kafka's arbitrary 50 MB decompression cap with 2^22, matching the Kafka Java LZ4 implementation's actual maximum. When a protocol or canonical implementation supplies a meaningful ceiling, prefer it over a convenience number and document the source. This complements later resource-limit guidance distinguishing legal logical size from unsafe resource commitment.

## Return the semantic result callers need

In !5654, Guy Harris changes a Kafka helper from returning raw string offset/length metadata to returning the escaped display string its sole caller needs. Using `proto_tree_add_item_ret_display_string()` ensures the caller's appended text and the protocol-tree rendering share one decoding/escaping path. Avoid making callers reconstruct semantic/display values from low-level positions when the common decode layer can return the intended result directly.

## Use standard logging interfaces instead of private per-tool debug modes

John Thacker's merged !5642 removes text2pcap's private repeatable `-d` debug switch and maps its two effective levels to Wireshark DEBUG and NOISY logging. Shared logging usage/help becomes authoritative, docs/tests are updated, and `-q` remains a separate “suppress normal summaries” behavior. CLI quietness and diagnostic log filtering are different contracts.

## Prefer scoped proto-data over globals, and distinguish add from set semantics

Gerald Combs's merged !5640 introduces public `p_set_proto_data()`: matching packet/file-scope protocol data is updated in place, otherwise added. The API documentation explicitly positions proto-data as a way to share packet-scoped state between dissectors or persist file-scoped state without globals. Choose the lifetime scope from the stored data, and choose add-versus-set based on whether duplicate entries are meaningful.

## Bound file-format recognition separately from full record parsing

Merged !5635 makes RFC 7468 reading complete enough for multiple structures and arbitrarily long logical lines, with checked aggregate lengths, but deliberately keeps format recognition to a small initial probe. File open/recognition should be bounded and cheap even when committed record parsing must support a much larger legal input domain.

## Do not weaken mandatory packaging checks just to make CI green

During merged !5633, Gerald Combs rejected making the Windows C-runtime redistributable optional because an installer built without it could fail immediately on users' machines. The correct fix was to repair the CI image/environment and artifact discovery. A green packaging job is not useful if it can silently omit a runtime prerequisite.

## New TCP dissectors must be stream-correct

Jaap Keuter's review of merged !5624 calls out that TCP has no packet/PDU boundary guarantee and directs the new OCP.1 dissector to Wireshark's TCP reassembly/desegmentation guidance. The same review catches uninitialized `hf_` fields, encoding/type inconsistencies, wrong return-type values, and stale `_U_` annotations; Alexis La Goutte also requests a real capture. These are strong corroboration of the existing new-dissector checklist.

## Time-zone test expectations must be independent of parser arithmetic

Merged !5612 added multiple ISO-8601 offset syntaxes and regression tests, but both code and tests used the same reversed sign convention. John Thacker's later merged !5668 fixed both. Expected instants for sign-sensitive time conversion should come from independently known UTC equivalences, not from duplicating the algorithm under test.
