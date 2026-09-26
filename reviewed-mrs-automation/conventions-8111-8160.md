# Durable conventions from MR batch !8111-!8160

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

This batch contains 48 merged MRs and two closed MRs. The rules below come from merged master work unless explicitly qualified; Guy Harris-authored or reviewed changes are treated as especially strong evidence.

## Registration should carry the semantic context a generic consumer needs

- **Do not infer an application's transport contract from nearby entries in `pinfo->layers`.** In !8118, Guy Harris replaced CoAP's previous-layer inspection with distinct entry points for WebSockets, byte-stream transports, and other transports. Each wrapper passes an explicit transport enum to common code. MPTCP is a concrete reason the previous-layer heuristic is unreliable.
- **Generic UI should consume optional capabilities from registration instead of hard-coding protocols.** In !8122, John Thacker adds an optional stream-count callback to Follow Stream registration and removes TCP/UDP/DCCP-specific count dispatch from the Qt dialog.
- **When a generic protocol needs a context-specific interpretation, opt into that interpretation explicitly.** In !8135, Guy Harris adds a CoAP-for-TMF Decode As entry point plus a dedicated `coap_tmf_media_type` table instead of globally redefining `application/octet-stream`.

## Keep dissector identity domains distinct

Guy Harris's !8126 and !8130 establish three separate concepts:

- the dissector handle's **description**, used for human-facing selection such as Decode As;
- the dissected protocol's **short name**;
- the dissected protocol's **long name**.

A single protocol can have multiple selectable dissector handles, so a protocol name cannot always describe the choice the user is making. Conversely, code that wants the protocol identity should not accidentally use a handle-specific description.

## Internal protocol-tree strings are UTF-8

John Thacker's merged !8121 makes the contract explicit: a value supplied directly to `proto_tree_add_string()` or its format-value variant must be a valid UTF-8, NUL-terminated internal string, regardless of the packet's original character encoding. Decode/validate packet bytes before passing them into the text API.

## Desegmentation and persistent analysis state remain conditional

- !8114 checks `pinfo->can_desegment` before TCP's reassemble-until-FIN path requests desegmentation. Seeing FIN does not grant reassembly capability.
- !8137 prevents TLS from updating multisegment-PDU end state on redissection. Persistent stream state should be mutated during the intended state-building pass, while later passes consume the result.

## Model key representation and membership semantics deliberately

- !8160 uses `GUINT_TO_POINTER` / `GPOINTER_TO_UINT` with direct hashing for frame/session IDs that are 32-bit and start at 1. This removes unnecessary file-scope allocations. Apply this only when the integer domain fits the pointer-encoding API and NULL/zero is not a needed stored identity.
- !8132's discussion explains why value lookup is not always a membership test: when zero is a legitimate stored value, a lookup returning zero cannot distinguish “present with value 0” from “absent”. Use an explicit contains API in that case.
- The same !8132 review moves repeated dissector state toward Wireshark-managed wmem/autoreset lifetime rather than manually managed GLib lifetime where wmem already models the scope.

## Wiretap and pcap link-type namespaces are separate

During !8149, Guy Harris explicitly corrected an attempted use of the pcap/pcapng LINKTYPE number as a `WTAP_ENCAP_*` value. Wiretap encapsulation IDs are Wireshark's internal namespace and must use the next available internal value; the external LINKTYPE-to-Wiretap mapping is a separate table.

## Generator changes imply regeneration obligations

In !8153, John Thacker called out that changing the GIOP `idl2wrs` generator requires regenerating the in-tree dissectors produced by that generator. A generator-only change is incomplete when committed generated consumers should change. This is corroboration for the stronger generated-code CI rules already recorded from later work.

## Review and submission corroboration

- !8155 shows the value of validating the entire specification-defined structure surrounding a reported bug; John Thacker noticed that the first fix still omitted the partial extended-header-list portion of the NAS IE.
- !8141 independently corroborates Wireshark's commit-message policy: component-prefixed concise subject, a blank line before body text, and lines kept under 80 columns.
- !8124 corroborates Gerald Combs's preference for typed Qt member-function-pointer `connect()` syntax rather than obsolete string/signature forms.
- Closed !8150 and !8147 are intentionally not treated as accepted implementation exemplars. Their discussion can inform design, but merged successors/current infrastructure carry more weight.
