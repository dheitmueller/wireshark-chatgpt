# Wireshark Capture and Decoder Boundary Conventions

This file records durable architecture conventions about where capture, transport, and protocol interpretation responsibilities belong. Current upstream source remains authoritative.

## Preserve raw structured capture data and perform semantic formatting in the decoder

Capture helpers should collect and transport the information needed for dissection without prematurely collapsing structured source data into display-only text. When semantic formatting belongs to protocol interpretation, doing it in the dissector/parser keeps the underlying properties available as independently filterable fields.

Merged MR !24666 refactors ETW support so etwdump transports raw event properties and extended data while user-message formatting moves into the ETW parser. The MR explicitly identifies the benefit: unformatted properties remain visible and filterable rather than being consumed by formatting inside the capture helper. Anders Broman approved and merged the change.

**Architecture rule:** keep acquisition helpers focused on obtaining and faithfully conveying source data. Put semantic decoding and display formatting in the parser/dissector when that preserves structured fields, filterability, and reuse. Do not make an acquisition layer the only owner of an interpretation that downstream analysis still needs in raw form.

**Confidence:** High. Substantial merged master refactor approved by Anders Broman, with the architectural motivation stated explicitly in the MR.

## Decode shared protocol structures in their canonical protocol implementation

A specialized encapsulation or transport should not keep a private copy of a protocol block decoder once the same block format is shared with other paths. Centralize the wire-format interpretation in the protocol implementation that owns that structure, and let specialized paths dispatch into it.

Merged MR !24688 replaces an earlier proof-of-concept where SXP PNIO blocks were dissected directly in `packet-pn-sxp.c`. The accepted implementation moves PNIO block dissection into the common PNIO implementation so SXP and non-SXP packets use the same decoding logic. Anders Broman approved and merged the result.

**Architecture rule:** once a wire structure is common across encapsulations, maintain one authoritative decoder at the protocol layer that owns the structure. Wrapper/specialized dissectors should supply framing or context and reuse that decoder rather than forking equivalent field parsing.

**Confidence:** High. Merged master refactor with explicit deduplication rationale and Anders Broman approval.
