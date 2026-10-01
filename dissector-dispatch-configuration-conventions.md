# Wireshark Dissector Dispatch and Configuration Conventions

This file records durable conventions for dissector dispatch mechanisms and user-selectable decoding behavior. Current upstream source remains authoritative.

## Do not partially migrate a dissector-table contract while shipped consumers still depend on the old contract

Changing a dissector table or dispatch contract is an ecosystem migration, not merely a registration-site refactor. If Wireshark's own registered consumers still expect the previous payload shape or context, introducing the new dispatch path can break those dissectors even when the new table itself is internally consistent.

Merged MR !21083, authored and merged by John Thacker, reverted an attempted split of Bluetooth HCI vendor Command and Event payload tables. The earlier change had not converted the vendor dissectors shipped with Wireshark and also changed which Event bytes were passed to them, so those dissectors could no longer distinguish all event forms they were expected to handle. The accepted response was to restore the compatible table contract and document the ambiguity until a complete migration can be designed. Release-4.6 backport !21086 preserves the same decision.

**Implementation rule:** before changing a dissector-table payload/context contract, enumerate the registered in-tree consumers and migrate them coherently. Do not land infrastructure that assumes a new contract while existing consumers still depend on the old one. If a complete migration is not ready, preserve the working contract and document the architectural problem rather than leaving a half-converted dispatch graph.

**Confidence:** Extremely high. Merged master architectural correction and stable-branch follow-up authored by John Thacker.

## Prefer generic Decode As selection over a one-vendor preference that duplicates it

When an existing dissector table already exposes the desired user override through Decode As, do not add or retain a protocol preference that hard-codes one particular downstream dissector. The generic dispatch mechanism scales to all registered choices and avoids parallel configuration paths for the same semantic decision.

Merged MR !21065, authored by John Thacker and merged by Michael Mann, removes the Bluetooth HCI `Dissect all vendor cmds as Android` preference because the vendor payload table already supports choosing Android—or any other vendor dissector—through Decode As. John explicitly notes that the special preference duplicated the table's behavior and complicated the code.

**Implementation rule:** before adding a boolean or enum preference to force a particular subdissector, check whether the relevant dissector table already provides Decode As for that selection. Prefer the generic mechanism unless the preference represents genuinely different protocol semantics rather than another spelling of the same dispatch choice.

**Confidence:** Very high. Merged master simplification authored by John Thacker and accepted by Michael Mann.
## Explicit user overrides take precedence over default transport dispatch heuristics

Merged master MR !4206, authored by John Thacker, changes TCP, UDP, and SCTP dispatch when both endpoint keys have registered dissectors. The accepted code first detects registrations changed from their defaults by Decode As or a preference and tries those explicit choices before the ordinary server/lower-port ordering. Only unchanged/default entries fall back to the historical heuristic order.

**Implementation rule:** when a user has explicitly changed a dissector-table binding, honor that choice before applying default heuristics such as lower-port or server-port preference. User configuration is stronger evidence than the framework's guess about which endpoint represents the application.

**API rule:** if dispatch needs to distinguish an explicit override from a default registration, represent that distinction in the dissector-table API rather than reverse-engineering it independently in each transport.

**Confidence:** Very high. Merged cross-transport framework change authored by John Thacker.


## Keep overlapping numeric identifier domains in separate specific tables

Merged !3668 adds separate `can.id` and `can.extended_id` tables because standard CAN IDs and extended CAN IDs can have the same numeric value while belonging to different semantic domains. Those specific tables are tried before the pre-existing generic `can.subdissector` mechanism, which is retained as a compatibility fallback. Follow-up merged MRs !3682, !3684, and !3685 propagate the same dispatch API to other CAN carriers/consumers.

**Dispatch rule:** if two protocol identifier spaces overlap numerically but have different wire semantics, do not collapse them into one keyed table. Use separate domain-specific tables and route according to the decoded identifier class before lookup.

**Compatibility rule:** when an older generic extension point is already in use, preserve it as a fallback unless there is a deliberate migration plan. New more-specific registrations may take precedence without silently invalidating existing Decode-As or plugin registrations.

**Confidence:** Very high. Merged master API design with multiple immediate merged adopters.


## Attempt explicit payload mappings before heuristic dissectors

Merged MR !2681 fixes MQTT subdissector selection. UAT-configured payload mappings and media-type dispatch are attempted first, their return values determine whether the payload was actually handled, and heuristic dissectors run only if neither explicit path succeeds. This corrects the earlier !2670 placement where heuristics were tied only to absence of a UAT match and could bypass the media-type decision.

**Dispatch rule:** order candidate decoders from strongest explicit configuration/protocol metadata to weaker heuristics. Track whether each dispatch path actually accepted the payload; do not equate “a lookup path existed” with “the payload was handled.”

**Confidence:** High. Merged master correction of the immediately preceding heuristic extension.
