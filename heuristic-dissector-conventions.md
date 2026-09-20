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
