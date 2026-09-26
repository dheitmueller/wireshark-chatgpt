# Durable conventions from !8561–!8610

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

## Text decoding

Merged !8604 (John Thacker) shows that layered wire encodings must be decoded in protocol order. UCP/EMI represents GSM 7-bit alphabet octets as ASCII hexadecimal pairs, so the correct path is ASCII hex -> bytes -> GSM alphabet, with malformed hex handled before character-set conversion.

Merged !8569 (John Thacker), after explicit review from João Valverde, keeps generic `wmem_strbuf_append_unichar()` semantics separate from a new validated append operation used by charset decoders. Invalid Unicode scalar values become U+FFFD, and incomplete terminal UCS-2 input is represented as malformed rather than silently discarded.

Merged !8574 (João Valverde) establishes that a length-delimited string validator must not inherit C-string termination semantics accidentally: `wmem_strbuf_utf8_validate()` continues validation across embedded NUL bytes, with focused tests for NULs before and around invalid sequences.

Closed !8570 proposed global ASCII validation in DNS-name expansion, but John Thacker pointed out that Microsoft LDAP/Netlogon reuse the DNS structural encoding while documenting UTF-8 text. João replaced that design with merged !8572, which keeps `get_dns_name()` encoding-agnostic and makes each caller apply its own charset rules.

**Rules:** decode representation layers in wire order; keep generic string-building primitives distinct from validated text conversion; treat embedded NUL according to the counted-vs-terminated contract; and keep structural parsers charset-neutral when callers legitimately use different text encodings.

## Conversation identity

Merged master !8571 (John Thacker), with release-4.0 backport !8600, changes SRTCP lookup and creation to use the supplied `setup_frame_number` rather than the frame currently being dissected. This ensures RTP and RTCP registrations created from the same negotiated setup converge on the same conversation.

**Rule:** for negotiated flows, the frame component of conversation identity is the frame that established the mapping, not necessarily the current callback frame.

## C portability and parser progress

Merged !8587 fixes Win64 tests by using fixed-width `guint64` constants for 64-bit shifts. Windows is LLP64, so `unsigned long` is still 32 bits in a 64-bit process. Closed !8586 and !8580 are superseded variants of the same correction.

Merged !8602 (John Thacker) fixes end-offset contracts: an unpaired final hex digit is inspected but not consumed, and a failed `tvb_get_string_bytes()` conversion initializes `endoff` to the parse start rather than unrelated absolute zero.

**Rules:** use fixed-width constant forms when expression width matters across LP64/LLP64 targets; parser end positions report complete consumed units, and no-progress failure offsets should remain in the caller's coordinate system.

## CI ordering

Merged !8592 (Gerald Combs) makes expensive merge-request build jobs depend on Commit Check so quick deterministic failures stop the pipeline before consuming additional runner capacity.

**Rule:** encode cheap high-signal preflight checks as dependencies of expensive platform/compiler jobs when the latter add no useful value after preflight failure.

The remaining per-MR evidence, including negative follow-up evidence from !8597 and new-dissector review patterns from !8577–!8579, is recorded in `reviewed-mrs-automation/review-findings-8561-8610.md`.
