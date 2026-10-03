# Subdissector Context Conventions from !1010-!1059

## Carry the semantic discriminator the nested decoder actually needs

Closed WIP !1017 attempted to expose broader S1AP private state and copy a generic outer message type from NGAP. Merged master !1019 instead records a focused `transparent_container_type` distinguishing source-to-target from target-to-source containers, sets it in the authoritative ASN.1/conformance callbacks, and selects nested RRC decoding from that value. The generated C is regenerated from those inputs.

**Rule:** prefer a narrow semantic discriminator over exposing a broad private context or using an outer message category as a proxy. For generated dissectors, make the state assignment in the generator input, template, or conformance layer so regeneration preserves the architecture.

**Confidence:** Very high. The broad WIP was closed; the focused replacement merged on master and was backported in !1020 and !1021.
