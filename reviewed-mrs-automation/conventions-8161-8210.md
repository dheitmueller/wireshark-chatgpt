# Durable conventions from Wireshark MRs !8161-!8210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Text and encoding boundaries

1. **Do not collapse opaque bytes and decoded text into one value.** If downstream code legitimately needs the original protocol octets, keep those bytes separately while storing a validated/decoded string in text-facing fields or APIs. Merged !8204 does exactly this for HTTP header values.
2. **Decode stacked or structured text encodings through shared helpers.** Use Wireshark's encoding machinery such as `ENC_APN_STR` instead of rewriting length-label bytes in place. Merged !8199 is the concrete PFCP example.
3. **Honor the exact protocol encoding contract.** A generic Unicode/ASCII mode flag must not override a command-specific rule that says a field is OEM-only or otherwise fixed to a particular character set. Merged !8210 demonstrates this in SMB.
4. **Output formats have their own legal-character rules.** Valid UTF-8 is not sufficient to make a string legal XML. Merged !8184 handles the XML 1.0 ASCII-control restrictions explicitly.

## Qt signal and event handling

Prefer typed explicit signal/slot connections over connection-by-name or legacy string-based wiring. Choose the connection type with event lifetime in mind: if an action can destroy its context menu, run a nested event loop, or otherwise invalidate objects still involved in dispatch, queued delivery can be required. Merged !8171 is the strongest example in this batch, reinforced by !8196, !8197, and !8209.

## Dissector recognition and registration

A dissector associated with a non-standard/unassigned port must still reject unrelated traffic cleanly. Guard every recognition read, return without claiming on a mismatch, and only establish conversation ownership after sufficient positive evidence. Merged !8165 provides early John Thacker-authored evidence; the later Guy Harris-reviewed !8356 remains the stronger registration-policy authority already recorded elsewhere.

## Field semantics and source ranges

- Use `FT_CHAR` for semantically one-character values and keep language bindings consistent with native field behavior (!8190, !8178).
- A calculated/postdissector protocol item should claim only the bytes it actually represents; when it represents no source bytes, a zero-length range is correct (!8177).
- Distinct semantic values with different types must not share one ambiguous display-filter field identity. !8203 review explicitly pushed dynamic SAPHDB values toward type-specific filter names.

## New-dissector submission review

Review a new dissector as an integration, not just a parser. The !8203/!8202 review sequence checked:
- the standard source template and declaration/linkage conventions;
- field types, masks, and filter identities;
- visibility of reserved/unknown bytes;
- static-analysis and compiler warnings;
- release-note integration;
- registration defaults and Decode As/heuristic implications;
- contributor CI actually running;
- and representative packet captures.

## Stateful lookup performance

When protocol state is keyed by a structured identity, use the structured identity directly. Avoid repeatedly converting addresses/IDs to strings and then linearly scanning secondary lists. In merged !8163, changing GTP session tracking to a native TEID/address map produced a reported greater-than-10x speedup on representative captures.

## Exported-symbol packaging

Adding a public exported symbol is incomplete until package ABI manifests list it under the correct library/version. Merged !8208 and !8162 are direct examples and corroborate the existing ABI notebook rule.
