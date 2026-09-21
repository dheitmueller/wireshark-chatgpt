# Dissector Entry and Stateful Identity Conventions

These conventions capture durable lessons about how dissectors are entered, how protocol state is keyed, and how uncertain wire semantics should be represented.

## Do not assume dissector-table dispatch metadata exists

- A dissector that can be reached through multiple mechanisms must not assume metadata supplied only by one dispatch path is initialized. Merged !20218 factors SSH direction determination away from scattered direct uses of `pinfo->match_uint`, explicitly noting that `match_uint` is unsuitable for heuristic dispatch and can also be unsuitable for SCTP when PPI rather than a standard port selected the dissector.
- Merged !20223 then adds SSH heuristic dissection and reiterates that `match_uint` is not set when the dissector is selected heuristically or through conversation matching. When the protocol supports those entry paths, determine role/direction from protocol state or packet semantics instead of treating table-match metadata as universally available.
- More generally, when adding a new entry path to an existing dissector, audit assumptions that came from the old dispatch mechanism: matched port/value, `data` pointer contract, conversation state, minimum-length checks, and inferred direction may all differ.

## Key state by the complete protocol relationship

- Persistent protocol state must be keyed by every endpoint property that determines where that state is valid. Merged !20217 fixes Bluetooth ATT MTU tracking by including the remote BD_ADDR in the key: an ATT MTU belongs to a particular client/server pair, not merely to the local adapter/interface. A key that omitted the remote device allowed one peer's negotiated state to affect another.
- Prefer the protocol's actual conversation/session identity over a convenient partial key. If several parallel trees/maps all reproduce essentially the same conversation key, consider whether normal Wireshark conversation state can express the ownership more directly.
- Merged !20235 independently reinforces avoiding redundant state models: its review removed an extra gating mechanism because the existing GTP session tracking already supplied the needed lifetime and association.

## Use reported and captured length for their distinct meanings

- Reported length describes the original protocol packet; captured length describes bytes actually available in this capture. Do not substitute one for the other merely because both are lengths.
- Merged !20227 changes HSFZ's declared-message-length comparison to use reported length. This avoids treating a snaplen-truncated capture as though the protocol itself declared a shorter message. Captured-length APIs remain the right choice before actually reading bytes from the tvbuff.

## Treat reverse-engineered field semantics conservatively

- Captures can establish that bytes exist and can strongly suggest structure, but readable patterns alone are weaker evidence for assigning protocol semantics, names, or endpoint roles when the specification is unavailable.
- !20198 was merged after decoding HSFZ fields inferred from captures, but later protocol-expert review in !20216 stated that some inferred meanings were incorrect; !20216 itself was ultimately closed rather than merged. This is useful evidence that a visually plausible interpretation can still assign the wrong semantics.
- For an undocumented or proprietary protocol, separate observation from interpretation. Prefer neutral/unknown field naming until the meaning is supported by multiple independent captures, implementation behavior, specification text, or knowledgeable protocol review. Do not present a guess as normative wire semantics.

## Prefer authoritative registration tables over parallel switches and indexes

- When a registry/table already describes a family of wire objects, make it the authoritative source for operations that are properties of those objects instead of maintaining parallel switch statements or manual index constants.
- Merged !20189, authored and merged by Guy Harris, moves standard pcapng block behavior into `pcapng_block_type_information_t`, replaces several block-type switches with table lookup, stores option-table and internal-block properties in the registration record, rejects duplicate block-type registration, and removes manually maintained `BT_INDEX_` values.
- Table-driven registration reduces synchronization bugs: adding or changing a type should update one authoritative description rather than requiring matching edits to a registry, switch statements, index enums, and option tables.

## Register a dissector-table key only when the key actually names that protocol

A registration in a single-value dissector table is a semantic ownership claim, not a convenient fallback for a similar protocol. Competing registrations for the same key can displace one another, so registering a dissector under an encapsulation or selector that does not actually denote it can silently route valid traffic to the wrong dissector.

Merged master MR !14057, authored and merged by Guy Harris, removes the X.75 dissector's registration for `WTAP_ENCAP_LAPB`. X.75 uses LAPB-like framing, but a capture whose encapsulation is explicitly LAPB should be handed to the LAPB dissector; registering X.75 on that same `wtap_encap` value caused the two dissectors to compete for one selector. Merged release-4.2 backport !14058 carries the same correction.

**Registration rule:** before calling `dissector_add_*()` for a fixed selector, verify that the selector's documented semantics uniquely identify the protocol being registered. Protocol similarity is not sufficient. If one selector can legitimately represent multiple interpretations, use a distinct encapsulation, Decode As, heuristic selection, or another explicit dispatch layer rather than registering multiple unrelated dissectors as though they own the same value.

**Review rule:** when adding or changing a fixed dissector-table registration, check the existing registrations for that table/key and ask what the key itself promises. Treat a duplicate or overlapping registration as a potential correctness conflict, not merely a discovery convenience.

**Confidence:** Extremely high. Merged master correction authored and merged by Guy Harris, with an accepted release backport.