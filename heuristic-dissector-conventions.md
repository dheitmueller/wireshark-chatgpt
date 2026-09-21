# Heuristic dissector conventions

## A heuristic must have protocol-identifying evidence

Do not register a dissector as heuristic when its test is effectively only a likely port and packet size and the protocol has no reliable on-wire discriminator. Such a test risks claiming unrelated traffic while giving users little control over the ambiguity.

Merged MR !20903, authored by Michael Mann and approved by Anders Broman, converted DCC away from a UDP heuristic because the old heuristic checked only port/size and there was no real identifying data in the packet. The replacement used a configurable UDP port preference even though the port is not IANA-registered. The same change adopted `udp_dissect_pdus()` so multiple DCC messages in one UDP datagram follow the standard PDU-looping machinery.

Practical guidance:

- Use heuristic registration only when packet content supplies a sufficiently discriminating signature or structural test.
- When identification fundamentally depends on deployment configuration such as a non-standard port, prefer a configurable port binding / Decode As-capable dispatch path over a weak heuristic.
- Once selected, use the transport's standard PDU helper when the protocol permits multiple messages in one transport unit instead of open-coding message iteration.

## Re-evaluate heuristic defaults when accepted-value space expands

A heuristic that was selective enough when introduced can become unsafe after an otherwise legitimate protocol change broadens the values it accepts. Re-evaluate false-positive probability whenever timestamp precision, legal ranges, framing variants, optional fields, or similar changes enlarge the heuristic's acceptance space. If the discriminator becomes weak, disabling the heuristic by default is preferable to silently increasing false positives while a stronger test is sought.

Merged MR !19011, authored by John Thacker and merged after Alexis La Goutte's approval, is a concrete example. HiPerConTracer's timestamp plausibility test had accepted only about 0.01437% of the 64-bit search space when timestamps were constrained to microseconds. Adding nanosecond timestamps expanded that accepted space to about 14.37%, making UDP/TCP false positives too likely. The accepted response was to keep the heuristic available but disable it by default until a more discriminating test exists.

Practical guidance:

- Treat heuristic selectivity as a property that can regress when the protocol's valid input domain changes.
- Where practical, quantify the accepted-value space or otherwise estimate false-positive risk rather than relying only on captures that happen not to collide.
- A protocol feature expansion should trigger review of existing heuristic assumptions even if the heuristic code itself is unchanged.
- When a formerly safe heuristic becomes weak, preserve explicit opt-in availability if useful, but do not keep it enabled by default merely for backward compatibility.

## Heuristics that override explicit protocol metadata should be opt-in

When a protocol carries an explicit dispatch hint such as HTTP `Content-Type`, prefer that declared metadata by default. If real deployments frequently provide incorrect or overly generic metadata and a content heuristic can improve dissection, expose heuristic-first behavior as a user preference rather than silently changing the default precedence.

Merged MR !19235, authored by John Thacker, added optional HTTP MIME sniffing. The accepted implementation keeps `Content-Type` dispatch first by default, provides a preference to try body heuristics first, and documents why the heuristic-first mode is disabled by default in light of RFC 9110 guidance. It also preserves and restores `pinfo->can_desegment` around heuristic probing so changing dispatch order does not leak temporary packet-state changes into later dissection.

Practical guidance:

- Treat explicit on-wire dispatch metadata as authoritative by default unless the protocol specification or established Wireshark behavior says otherwise.
- If heuristic inspection is useful specifically to override unreliable metadata, make that precedence change explicit and user-configurable.
- Choose the default with protocol interoperability and specification guidance in mind, not merely whichever path produces the richest dissection on a sample capture.
- When probing a heuristic requires temporarily changing `packet_info` or other shared dissection state, save and restore that state around the probe.

## Compatibility relaxations that weaken a heuristic should be opt-in

Real implementations sometimes violate a protocol requirement in a way that users still need Wireshark to decode. If accepting that nonconformance makes a heuristic materially less selective, keep the standards-compliant validation as the default and expose the compatibility relaxation as an explicit preference rather than weakening recognition for everyone.

Merged MR !15877, authored and merged by John Thacker, handles ESP NULL-encryption implementations that use padding forbidden by RFC 4303. Wireshark had begun validating the required padding in its NULL heuristic; the accepted compatibility fix adds preference choices that can also accept all-zero or arbitrary padding, but deliberately defaults to the RFC-compliant check because looser padding increases false positives. Merged !15895 independently reinforces the same recognition philosophy by adding several cheap Ethernet structural checks to a pseudowire heuristic instead of accepting more ambiguous traffic.

Practical guidance:

- Distinguish interoperability tolerance from protocol recognition policy: a broken peer can justify an opt-in compatibility mode without justifying a weaker default heuristic.
- Keep the strict/default path aligned with normative protocol constraints when those constraints provide useful discrimination.
- When adding a relaxation, document the false-positive tradeoff so the preference is not mistaken for an equivalent validation mode.
- Prefer an enum or similarly explicit policy setting when there are meaningful levels of tolerance rather than a single opaque on/off switch.

## Use established conversation state to strengthen otherwise-weak encrypted-header heuristics

A newer protocol version can remove or encrypt the cleartext fields that made stateless recognition selective. Do not compensate by broadening the stateless heuristic until common multiplexed traffic also matches. If the packet is only useful after a session has already been identified, previously established conversation state can itself provide the missing recognition evidence.

Merged MR !15477, authored and merged by John Thacker, adds heuristic support for the DTLS 1.3 unified header. The first-octet space is deliberately compatible with RTP/RTCP/STUN/TURN/ZRTP/DTLS multiplexing and the legacy version bytes are encrypted, so the old cleartext checks are unavailable. The accepted heuristic therefore succeeds only when Wireshark has already detected a DTLS session on that connection and, when the unified header carries a connection ID, that CID matches the stored session. John also notes that without an established session Wireshark would not be able to decrypt the packet anyway.

Practical guidance:

- When encryption or compact framing removes a strong cleartext discriminator, do not replace it with a much broader stateless signature merely to retain heuristic coverage.
- Require previously established protocol/conversation state when that state is necessary to interpret the packet meaningfully anyway.
- Match stable session identifiers such as connection IDs in addition to coarse transport identity when the protocol supplies them.
- If neither packet-local evidence nor established state can distinguish the protocol reliably, decline the heuristic rather than claiming ambiguous traffic.

**Confidence:** Very high. Merged master change authored and merged by John Thacker, with the false-positive/multiplexing constraint and the state requirement explained directly in the MR.

## A heuristic's default policy must account for execution cost as well as false-positive risk

A content heuristic can be semantically sound yet still be a poor default if it is expensive and registered at a broad dispatch point where it may run against large volumes of unrelated traffic. Default enablement is therefore a performance policy as well as a recognition-policy decision.

Merged master MR !14463 adds an LTP heuristic while extending cancel/cancel-ack correlation. Anders Broman explicitly questioned enabling the heuristic by default because it could be CPU intensive; the author agreed and changed the registration before the MR was approved and merged. The review is useful because the concern is independent of whether the heuristic ultimately recognizes LTP correctly: a globally or broadly invoked candidate test can impose cost on captures that do not contain the protocol at all.

**Implementation rule:** when adding a heuristic, consider expected invocation frequency and worst-case parsing cost before choosing `HEURISTIC_ENABLE`. If the check requires substantial parsing, repeated lookups, or other nontrivial work at a broad transport table, prefer opt-in/default-disabled registration unless there is strong evidence the cost is negligible. Where possible, first narrow dispatch using a stronger parent protocol, conversation state, or explicit binding.

**Review rule:** assess both selectivity and cost. Positive sample captures demonstrate correctness after a match, but they do not measure the aggregate overhead of running the heuristic on unrelated traffic; benchmark or reason about the no-match path when registration is broad.

**Confidence:** High. Direct Anders Broman review, author agreement/change, and merged result. The evidence establishes the default-policy principle even though exact acceptable cost remains protocol- and dispatch-context dependent.
