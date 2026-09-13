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
