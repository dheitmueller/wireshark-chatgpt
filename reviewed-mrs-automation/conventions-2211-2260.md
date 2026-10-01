# Durable convention synthesis — Wireshark MRs !2211–!2260

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

This synthesis gives merged master work the highest weight, then merged stable backports, then discussion in closed/superseded MRs. Direct maintainer guidance is called out where it materially strengthens the rule.

## Parser progress and unknown-but-valid extensions

Merged !2248 (with stable forms !2249 and !2257) is a clean parser-hardening example. An earlier infinite-loop fix stopped GQUIC tag parsing on any unknown tag. That avoided the loop but also rejected perfectly valid extension tags Wireshark did not yet understand. The accepted fix advances over the unknown tag using its validated declared length while separately retaining an overflow/progress guard on the accumulated offset.

Rule: malformed-input loop protection should prove bounded forward progress; it should not turn protocol extensibility into an error. If an unknown extension has a validated length, skip/preserve it and continue rather than aborting the rest of the structure.

## Packed fields should use Wireshark's bitmask APIs

In merged !2253, Anders Broman explicitly asks the contributor to replace manual packed-flag handling with proto_tree_add_bitmask(); the accepted revision does so.

Rule: for a container byte/word made of registered flag fields, prefer the standard bitmask helpers and an hf_ field array over local repetitive extraction code. It keeps masks, display, highlighting, and field registration aligned with normal Wireshark patterns.

## Field width is a specification contract, not just a helper-call detail

Merged !2255 was prompted by tools/check_typed_item_calls.py: code added two bytes for a field registered as FT_UINT8. Martin Mathieson did not simply silence the checker; John Thacker checked the protocol specification and confirmed the field is actually 16-bit, leading to FT_UINT16.

Rule: when the typed-item checker finds a wire-width mismatch, verify the authoritative protocol definition and repair the semantic field registration/call contract. The goal is not checker silence; it is agreement among wire width, registered type, extraction helper, and protocol specification.

## Regression tests outlive a particular implementation fix

Merged !2254 initially accompanied a TVBuff subset-search implementation fix. Gerald Combs noted that !2556 had already fixed the bug differently and asked the contributor to revert the duplicate implementation change while keeping the new tests.

Testing rule: preserve a focused regression test when the bug contract remains important even if another patch supplies the implementation fix. Tests should describe observable behavior—here, search offsets in subset/composite TVBuffs—not depend on which internal fix happened to land.

Closed !2250 adds lower-weight architectural guidance: direct generated-TVBuff/proto-tree assertions can be a useful white-box test style, but production symbols should not be given broader linkage merely to make tests reach them.

## One ambiguous discriminator should have one deterministic default plus Decode As

Merged !2243, authored by John Thacker, handles MPEG section table ID 0x3E, which legitimately corresponds both to generic DSM-CC private sections and the much more common DVB MPE interpretation. Registering both and depending on registration order is unreliable. The accepted design registers MPE as the single default and exposes Decode As so users/plugins can select DSM-CC or another private interpretation.

Dispatch rule: when one table key has multiple legitimate decoders, choose one deliberate default using protocol prevalence/context and provide Decode As or another explicit override mechanism. Do not encode precedence accidentally through registration order.

## Protocol state must follow the protocol, including captures that start mid-session

Merged !2244 contains unusually useful Pascal Quantin review of PDCP-NR deciphering. Out-of-order delivery is valid for NR PDCP and should not be hidden behind an optional preference. User-plane traffic may be decipherable from configured keys even when the capture omitted the earlier Security Mode exchange. Pascal also warns that robust COUNT/HFN derivation around retransmissions and sequence rollover must implement the protocol's state model rather than assuming ideal in-order traces.

State rule: do not turn standards-permitted behavior into a preference simply because the current implementation finds it inconvenient. Separate “state unavailable because the capture began late” from “protocol says this state cannot exist,” and model sequence/retransmission state according to the specification when it affects cryptographic or semantic decoding.

## GUI diagnostics need a real logging/presentation policy

Merged !2235 includes direct Guy Harris analysis of stderr behavior for GUI-launched Wireshark: it can disappear to /dev/null, go to a systemd journal socket, or land in a desktop-session file depending on platform/environment. Guy distinguishes temporary developer debugging, diagnostics users may need to attach to bug reports, dissector-programming failures, and ordinary user-actionable configuration errors.

Logging rule: do not treat stdout/stderr availability as a cross-platform GUI contract, and do not solve that by banning diagnostics. Route messages through a common facility appropriate to their audience/severity so user-actionable errors are visible, developer diagnostics are retrievable, and dissector bugs use packet/dissector reporting where appropriate.

Merged !2259's ws_debug() consolidation is consistent with this direction: centralize debug output policy rather than proliferating private print wrappers.

## Quoted error packets must not mutate live transaction state

Merged !2227 changes DNS so packets being dissected under pinfo->flags.in_error_pkt do not participate in ordinary request/response tracking or retransmission classification.

State rule: a protocol PDU quoted inside an ICMP/ICMPv6 error is evidence about another packet, not a new live transaction event. Nested error-packet dissection may present fields, but it should not create/update the same conversation/request-response state that an actually observed protocol exchange would.

## Unknown registry values should be out-of-band and validated once

Guy Harris's merged !2224 makes WTAP_FILE_TYPE_SUBTYPE_UNKNOWN equal to -1 and removes the synthetic “unknown” table entry. !2238 then returns that named sentinel from a failed UI lookup and has callers report the impossible condition. !2217 adds explicit bounds checks and restructures dumper initialization so a file type/subtype is validated before it is stored in the dumper and reused.

Registry rule: “unknown/invalid” should not occupy an ordinary valid registry slot when it is not a real entity. Use a named out-of-band sentinel, validate indices at the owning boundary, and structure later helpers around the fact that the stored value has already passed that validation.

## API names and plugin breakage can enforce structural contracts

Merged !2215, authored by Guy Harris, renames wtap_register_file_type_subtypes() to singular because one call registers exactly one subtype. The API break is also intentional: stale Wiretap plugins must rebuild and thereby expose old file_type_subtype_info layouts. Registration additionally rejects structurally useless/bogus entries instead of terminating the entire application.

API rule: name APIs after the actual cardinality/semantics they implement. When an internal/plugin contract has materially changed, a deliberate source/ABI break can be safer than preserving a misleading compatibility surface that allows stale plugins to compile or load incorrectly; pair that with validation and nonfatal rejection where a bad plugin should not crash the host.

## Semantic field types depend on whether bytes are intelligible

Merged !2237, authored by John Thacker, uses FT_ETHER for an MPE destination MAC only when address scrambling is absent; when scrambled, the same bytes are represented as raw FT_BYTES, expert info explains why they cannot be interpreted, and the scrambled payload is not passed to IP/LLC decoders.

Field rule: choose a semantic field type only when protocol state says the bytes carry that semantic value. If encryption/scrambling/encoding makes the value unavailable, preserve and highlight the raw bytes and report the condition instead of fabricating a semantic decode.

## Standard-library formatting is the supported baseline

Merged !2245 removes the old ws_snprintf compatibility wrapper and updates developer guidance to prefer C99 snprintf/vsnprintf; Guy Harris asks whether remaining g_snprintf calls need to exist at all. Guy-authored !2246 also shows that format-width macros must match the actual argument type and that direct column-formatting helpers are preferable to formatting into a temporary buffer only to append it.

C portability rule: use the standard C99 bounded formatting APIs on supported Wireshark platforms unless a specific API requires otherwise. Match format specifiers/macros to the actual C type and avoid needless intermediate string buffers when a Wireshark presentation API already accepts formatted arguments.

## Generated ASN.1 sources and generated output move together

Merged !2226, authored by Anders Broman, adds the RFC 4985 ASN.1 modules/configuration and the regenerated PKIX Qualified C/header output in the same change.

Generated-source rule: continue treating ASN.1/template/configuration inputs as authoritative and commit the corresponding generated output when it is version-controlled. A feature added only to generated C is not durable.

## Submission policy should fail as early as practical

Closed !2229 is lower-weight than merged implementation, but its review aligns with later accepted commit-hook work already in the notebook. Martin Mathieson and João Valverde favored retaining useful commit-message policy while moving cheaply checkable failures into a local commit hook so contributors discover them before remote CI fails; Gerald Combs discussed CI comments for warning-like feedback.

Submission rule: keep project policy enforceable, but run cheap deterministic checks locally/pre-commit where practical and let CI remain the authoritative backstop. Avoid making remote CI the first place a contributor can learn about a purely local style/metadata violation.

## Sample captures remain expected review material

Merged !2239 (Opus) and !2228 (TLS-SRP) both include direct Alexis La Goutte requests for a pcap; contributors supplied them. !2237 includes a purpose-built capture, and !2222 also supplies focused GQUIC samples.

Review rule: protocol/dissector submissions should include a small representative capture whenever feasible, especially when dispatch preferences, encryption state, malformed variants, or extension parsing are central to the change.
