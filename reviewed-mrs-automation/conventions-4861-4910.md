# Conventions from Wireshark MRs 4861-4910

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

## UAT setters must tolerate pre-validation input

Merged master MR 4905 fixes an ASAN-detected EPL overflow caused by an assumption about UAT callback order. The Qt UAT model invokes the field setter with an empty string while inserting a row before the check callback validates it. The accepted fix makes the setter safe on empty or invalid text and uses the same `hex_str_to_bytes()` interpretation in the setter and validator.

**Rule:** a UAT field setter is not entitled to assume its input has already passed the check callback. It must safely accept transient empty or invalid editor values, and setter/check parsing should share the same validity rules so the two callbacks cannot disagree about what is legal.

## Recognition failure must be side-effect free

Merged master MR 4901, authored by John Thacker, moves EPL's message-type validity check ahead of column changes and port mutation. A rejected packet now returns without having presented itself as POWERLINK or altered packet dispatch state.

**Rule:** perform the cheap protocol-ownership test before mutating columns, packet endpoint or port metadata, conversation state, or protocol-tree presentation. A heuristic or ambiguous dispatch path that returns "not mine" should leave the packet observationally unchanged.

## Draft-to-RFC compatibility can require carrying version state

Merged master MR 4908 updates DTLS Connection ID from draft code point 53 to RFC 9146 code point 54 while preserving the draft encoding. The final RFC also changes authenticated-data construction, so Wireshark records whether the deprecated extension was negotiated and follows the matching cryptographic layout. Closed MR 4903 proposed only the code-point update and was superseded once review identified the semantic difference.

**Rule:** when a draft protocol becomes an RFC, compare wire semantics as well as assigned values. If old captures remain distinguishable, preserve the deployed draft variant with explicit version or state and route downstream parsing or crypto through the correct semantics; do not alias two numeric values when their payload or authentication rules differ.

## Generated dissector automation must fail safely

Merged master MR 4873 converts ASTERIX to a committed generated dissector plus in-tree template and update tooling. The generated C remains version-controlled for reproducible builds. Gerald Combs explicitly required the updater to fail without modifying the tracked output before it could join the weekly update job; follow-up MR 4957 implemented the safe update behavior and the automation was then enabled. Jaap Keuter's caution also led to an initial period of manual validation.

**Rule:** for scheduled regeneration from an external specification, keep a reproducible in-tree generation path, validate generated output, and make the updater transactional: failure must leave the previous known-good tracked artifact untouched. Prove the path manually before enabling unattended updates.

Merged master MR 4906 independently records the failure mode this avoids: the Skinny generated C had accumulated manual edits and had to be resynchronized with its XML, template, and generator inputs.

## Display-filter syntax migration is staged language evolution

Merged master MR 4862 adds comma-separated set syntax while retaining the old whitespace form; stable MR 4874 carries it to 3.6. Merged master MR 4881 later removes the deprecated whitespace separator and updates scanner, grammar, tests, release notes, User's Guide, and shipped examples. MR 4871 separately reviews the broader user documentation for recent filter-language changes.

**Rule:** evolve a user-facing filter grammar as a compatibility sequence: introduce and test the replacement syntax, document and deprecate the old form, then remove it with negative tests and synchronized user documentation and examples. Parser code alone is not the whole compatibility surface.

## Lexical and semantic validation should reject invalid constructs at their owning layer

Merged master MR 4864 removes the scanner's arbitrary-character catch-all and turns otherwise unmatched characters into scanner errors; it also tightens CIDR, float, and range token patterns so `..` is not accidentally swallowed. Merged master MR 4880 fixes a crash by giving `matches` an explicit semantic validator for field, function, and range LHS forms and a clean type error for unsupported operands.

**Rule:** lexical impossibilities should fail in the scanner rather than being forwarded as generic unparsed text, and operator-specific type constraints should fail during semantic checking rather than reaching assertions in execution. Add regression tests for accepted ambiguous punctuation and rejected operand classes.

## Prefer the standard Decode As preference binding over custom handoff bookkeeping

Merged master MR 4870 replaces BSSAP+'s numeric preference plus handoff callback and re-registration logic with `dissector_add_uint_with_preference()`. The legacy preference is marked obsolete; the standard Decode-As-capable table binding carries the configurable default without an apply callback.

**Rule:** when a preference merely selects the default key for a dissector-table binding, prefer the table's standard with-preference registration API instead of maintaining old and new values and deleting and re-adding registrations in a preference handoff callback.

## Packet-controlled element lengths must guarantee progress

Merged release-3.6 MR 4872 breaks the ORAN section-extension loop when the packet supplies reserved `extlen == 0`, after adding expert information. Without the break the offset never advances and malformed input loops forever.

**Rule:** a malformed zero length that controls outer-loop advancement is a control-flow error, not merely a bad displayed value. Diagnose it and terminate that repeated parse path.

## Explicit user configuration can recover missing stream context

Merged master MR 4877 adds an HTTP/2 UAT for fake headers keyed by server port, stream ID, and direction so long-lived gRPC streams can be dissected when capture begins after the initial HEADERS frame. The MR includes an intentionally incomplete capture and regression coverage.

**Rule:** when partial captures legitimately omit state that cannot be inferred safely, prefer explicit, narrowly scoped user-supplied context over heuristics that manufacture state. Key the override by the identity dimensions needed to avoid contaminating unrelated streams, and test it with a capture that actually starts mid-session.

## Closed review evidence: generated translations and internal lifecycle APIs

Closed MR 4878 was rejected because the Qt translation file is synchronized from Transifex; the authoritative correction belongs in the translation service rather than the generated or synchronized `.ts` product. Closed MR 4894 was challenged because `proto_deregister_protocol()` is an internal part of the Lua reload lifecycle, not a standalone plugin API.

**Lower-weight rule:** change generated or synchronized artifacts at their authoritative source, and do not export an internal lifecycle primitive merely because an out-of-tree extension wants it; first establish a coherent supported lifecycle contract.
