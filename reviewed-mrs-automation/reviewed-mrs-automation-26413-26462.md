# Wireshark MR automation review: !26413-!26462

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection method: rebuilt the already-reviewed set from the per-run ledgers in `reviewed-mrs-automation/` plus `reviewed-mrs.md`, explicitly preserving/counting the historical !17571-!17620 batch. The prior run covered !26463-!26512, so the fifty highest-numbered corpus MRs not already reviewed are exactly !26462 down through !26413.

## Exact reviewed set (50)

!26462, !26461, !26460, !26459, !26458, !26457, !26456, !26455, !26454, !26453,
!26452, !26451, !26450, !26449, !26448, !26447, !26446, !26445, !26444, !26443,
!26442, !26441, !26440, !26439, !26438, !26437, !26436, !26435, !26434, !26433,
!26432, !26431, !26430, !26429, !26428, !26427, !26426, !26425, !26424, !26423,
!26422, !26421, !26420, !26419, !26418, !26417, !26416, !26415, !26414, !26413.

## Review notes

The batch is unusually heavy on defensive parser and arithmetic fixes plus backports. Strong examples include the SMB checked-add/clamp fix (!26449 master and !26462 release-4.6), OpenFlow hello-element minimum-length validation (!26433/!26444-!26459 family), KDP option minimum-length validation to guarantee parser progress (!26432/!26435), OpenVPN wrapped-client-key remaining-length validation (!26430/!26434), TSDNS short-packet guarding (!26431), USB HID inclusive-loop widening (!26417/!26428/!26429), SMPP/SCSI field-width corrections (!26414/!26416), and UBX bitmask encoding correction (!26421). These strongly corroborate existing notebook guidance on checked arithmetic, validating wire-derived lengths before subtraction/iteration, matching host integer width to protocol field width, and ensuring malformed TLV/option input cannot stall a loop.

!26440 fixes OBEX reassembly identity: packet direction alone is not a unique reassembly key. This reinforces the existing state/reassembly convention that keys must encode the protocol identity needed to distinguish concurrent logical objects rather than relying on a convenient but non-unique transport attribute.

!26439 removes floating-point and locale-dependent formatting from USB Audio BCD version rendering in favor of integer semantics. This independently corroborates the existing locale/serialization convention: protocol-defined numeric presentation must not depend on the process locale, and integer/BCD wire values should not be routed through floating-point formatting when exact digit semantics are intended.

!26427 changes the eCPRI/O-RAN handoff behavior so invoking a subdissector does not incorrectly suppress the caller's normal processing. This reinforces the existing dissector-composition rule that caller-visible return/claim semantics are part of the API contract; a nested dissector call should only terminate or hide parent processing when ownership was actually transferred by design.

!26426 adds an RTP-over-QUIC dissector, while !26413 adds HyperDHT/DHT-RPC support. Both were considered as new-protocol exemplars; their reusable lessons are already covered by the notebook's guidance on conservative recognition, protocol-specific dispatch, test captures, and keeping framing/transport responsibilities explicit.

!26448 hardens profile-name handling against path traversal outside profile directories. !26445/!26446 and !26453/!26454 fix error-path/update leaks. These are useful defensive examples but do not establish new Wireshark-specific conventions beyond existing input-validation and ownership/lifecycle guidance.

!26460 is the quickly closed/superseded PackClient Core submission; !26461 is the merged successor and therefore received greater evidentiary weight. Backports and superseded submissions in the batch were likewise weighted below their merged master counterparts.

No new convention text was added this run: the strongest durable findings independently corroborate conventions already present, and duplicating them would make the notebook noisier rather than more authoritative.
