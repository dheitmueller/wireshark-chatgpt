# Durable conventions extracted from MR review 4511–4560

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

Merged work receives primary weight. Stable backports corroborate master behavior. Closed or superseded MRs are used only for rejected/superseded design or review-history evidence. No substantive Guy Harris authored change or review comment appeared in this batch.

## Ordering API design — !4556

João Valverde's merged master change replaces six ftype callbacks for equality/inequality and the four ordering relations with one comparator whose sign encodes less/equal/greater. A durable design lesson is to represent one coherent ordering once and derive ordinary comparison operators from that contract rather than maintaining parallel callbacks that can drift.

## Semantic validity before child dispatch — !4552

TECMP FlexRay Null Frames can still carry bytes, but the protocol marks those bytes invalid. The merged fix suppresses downstream dissection when the Null Frame flag is set. Byte presence alone therefore is not permission for child dissection when the enclosing protocol has an explicit validity/absence signal.

## Independent configuration paths — !4544

ISO15765 previously let a missing LIN handle short-circuit CAN setup and incorrectly tied CAN table updates to the LIN diagnostic preference. The merged fix scopes LIN and CAN updates independently. Optional handles and preferences for one transport/subprotocol should not gate independent registration state.

## Bounded child tvbuffs — !4543

TECMP now creates child tvbuffs from the protocol-derived payload length rather than all bytes remaining in the parent. The remaining captured length is an availability bound; a child dissector should receive the semantic extent declared by the enclosing protocol.

## Frame-relative conversation dispatch — !4537

John Thacker's merged BT-DHT/uTP change uses `conversation_set_dissector_from_frame_number()` because the same UDP conversation can legitimately switch protocol ownership. A single timeless conversation handle made historical dissection depend on which packet the GUI had most recently visited. When dispatch changes over a conversation's lifetime, persisting the transition at a frame boundary keeps redissection deterministic.
