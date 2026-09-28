# Durable conventions from Wireshark MRs !6311–!6360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Timestamp validity gates timestamp formatting

`frame_data::has_ts` is part of the data contract. Code that may legitimately encounter a frame without a timestamp should check the validity bit before reading or formatting timestamp-derived state. Lower-level formatting helpers may assert that the precondition has already been satisfied, while a higher-level column setter should return an appropriate empty value for the no-timestamp case.

Evidence: merged !6355 and !6357, plus Guy Harris's release-3.4 implementation !6360.

**Rule:** separate an optional timestamp at the caller boundary from a programmer error inside a helper that requires one.

## Reassembly completion identity may require more than a frame number

Merged !6330, authored by John Thacker, requires both the reassembled frame number and recorded protocol-layer number before TCP treats the current occurrence as the final segment. Merged !6331 shows that frame+layer can still be ambiguous when two logical MPEG-TS fragment boundaries share the same frame/layer, so protocol-local `fragment_last` state is also used before dispatching the reassembled payload.

**Rule:** use every semantic discriminator needed to identify the exact logical reassembly occurrence. Frame number alone is not universally unique; frame+layer may still need protocol-local completion state.

## Defer protocol-layer creation until a complete PDU is actually dispatched

Merged master !6353, authored by John Thacker, moves HTTP/2 protocol-column and tree creation into the complete-PDU callback used by `tcp_dissect_pdus()`. Creating the layer earlier produced an empty HTTP/2 tree on the first pass when no full PDU was available, while later passes skipped that layer.

**Rule:** when a framing helper decides whether a complete logical message exists, delay protocol-layer creation and message-specific side effects until the callback for that complete message.

## Prefer typed array allocation and validate derived indexes

The large MariaDB/MySQL feature MR !6311 allocated one metadata array using the width of the wrong element type. Gerald Combs fixed this in merged !6340 with `wmem_alloc0_array(scope, element_type, count)`, while also validating the input count before storing it in a narrower representation. Merged !6345 then added a separate bounds check for the derived metadata index before array access.

**Rule:** express element type and element count separately with typed allocators. Validate external counts before narrowing or allocation, and independently validate derived indexes before access.

## Follow-up regressions outweigh the original merge bit

!6311 merged but immediately required !6340 and !6345 for correctness fixes. !6329 also merged, but later testing found a crash and higher-numbered follow-up work superseded its implementation.

**Rule:** when extracting durable architecture from MR history, later demonstrated regressions outweigh the fact that an earlier MR merged. Keep the earlier MR as historical or negative evidence rather than current precedent.

## WSLua generic field APIs need a consistent typed return contract

Merged !6343 makes `TreeItem:add_packet_field` return the item, decoded value, and next offset consistently across many supported field types, reusing native `proto_tree_add_item_ret_*` helpers and adding tests that compare results with direct `TvbRange` decoding. Roland Knall explicitly raised compatibility concerns about changing an established scripting API.

**Rule:** keep generic binding behavior consistent across field types, document every return value, test against the native primitive, and treat established scripting behavior as compatibility-sensitive.

## Explicit syntax should resolve lexical ambiguity

Merged !6334, authored by João Valverde, addresses tokens that can be interpreted as either registered names or literal values by providing explicit syntax for the intended semantic domain. Later display-filter MRs refine the current syntax and remain authoritative for today's exact behavior.

**Rule:** when lexical domains overlap, provide a direct way to express intent and carry that distinction through semantic analysis rather than relying only on lookup precedence.
