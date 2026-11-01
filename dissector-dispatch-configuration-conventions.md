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

## Resolve protocol namespaces sharing a transport at the actual service discriminator

**Merged master !9409** allows UDS and OBD-II requests on the same ISO-15765/DoIP connection without repeatedly changing Decode As. The UDS dissector recognizes service identifiers below its UDS minimum (`0x10`), and, under a separately named user preference, invokes a registered OBD-II handler on the appropriate payload subset. Merged !9408 also fixes a legal zero-parameter OBD-II service-0x04 request/response.

**Dispatch rule:** where two protocols legitimately coexist over one transport/session, distinguish them using the protocol-defined service discriminator and an explicit handler, rather than forcing the user to replace the entire transport's dissector selection. Keep a preference when it changes the interpretation of *overlapping protocol semantics*, rather than merely duplicating an existing table's Decode As choice. Test zero-parameter legal messages as well as ordinary nonempty payloads. **Confidence:** high, merged master implementation; no substantive maintainer discussion captured.

## Derive automatic port bindings from the actual advertised service

Merged master !9191 (Lars Völker) tightens SOME/IP Service Discovery's dynamic port registration. The presence of a generic endpoint option does not make that endpoint a SOME/IP service: options associated with the reserved `0xfffe` non-SOME/IP service identifier must not register its ports for SOME/IP. The same implementation exposes which endpoint options the legitimate entries reference and includes IPv6 address information.

**Dispatch rule:** a discovered address/port is not sufficient evidence to claim its traffic. Apply the protocol's service discriminator and option-reference relationship before installing automatic dissector bindings; otherwise a different application sharing the transport can be hijacked by an unrelated dissector. Validate both legitimate service entries and intentionally excluded service classes. **Confidence:** High, merged focused protocol correction.

Open, unmerged !9166's TPM20 reassembly rewrite illustrates a **separate review risk**, not an approved implementation: Martin Mathieson noted that command/response direction cannot safely rely on hardcoded TCP ports when registered ports are user-configurable. Prefer protocol-message discriminators or established per-conversation direction state where the wire protocol permits; inspect user preferences and Decode As entry. **Confidence:** High for Martin's objection, low for the open proposal as a model.

## Provide user-controlled embedded-payload selection through validated UAT rows

Merged master **!8599** adds an AMQP Message Decoding UAT modeled on MQTT. Its rows define an AMQP destination/topic matcher (exact, substring, prefix, suffix, or compiled regex), a named payload dissector, and optional additional data passed via `call_dissector_with_data()`. The code uses `UAT_AFFECTS_DISSECTION`, a callback to validate/compile regex entries, and an appropriate free callback for pattern, compiled-regex, and optional strings. The contributor supplied a sample AMQP capture, a Protobuf schema, and example UAT/ProtoBuf configuration needed to reproduce the dispatch.

**Rule:** where one transport carries user-specific application payloads that cannot be inferred from wire bytes alone, prefer a discoverable configured dispatch table rather than hard-coded application guesses. Define clear match semantics, verify dissector handles and callback data types, manage compiled-matcher lifetimes through UAT callbacks, and include an end-to-end capture plus configuration in the MR. **Confidence:** high for accepted AMQP UAT approach; no claim that every arbitrary payload protocol accepts the same additional-data convention.
