# Durable Wireshark conventions from MRs !8311–!8360

Corpus source for this batch: `dheitmueller/wireshark-corpus-mrs` at `ddcaa22b51c68f594e425a23388c3a2086813054`.

This file captures durable conventions extracted from the batch. Merged master MRs are weighted most heavily; stable-branch backports mainly corroborate master behavior; closed/superseded MRs are used only as negative evidence.

## Unassigned or non-exclusive ports should not be claimed by default

Merged master MR !8356 is strong review evidence. The WoWW dissector initially tried to bind TCP 8085 with `dissector_add_uint_with_preference()`, but 8085 is not IANA-assigned to WoWW and is also used by web servers. John Thacker flagged the false-positive/regression risk, and Guy Harris explicitly said not to register the dissector that way. Guy recommended `dissector_add_for_decode_as("tcp.port", ...)` unless a sufficiently reliable content heuristic could be developed. The contributor changed the merged submission to Decode As.

**Rule:** a customary but unassigned/non-exclusive port is not sufficient evidence for automatic protocol ownership. If a dissector cannot reliably distinguish its protocol from unrelated traffic, expose it through Decode As rather than claiming the port by default.

**Related rule:** direction logic must not assume the customary port once Decode As is possible; use the actual table match/conversation state.

**Authority:** extremely high; direct Guy Harris and John Thacker review incorporated before merge.

## Heuristic probes must reject safely and may be called again

The same !8356 review contains a precise heuristic contract. John Thacker noted that client and server recognition need different minimum lengths and that throwing a bounds exception from a heuristic is unacceptable. He also clarified that a heuristic returning `FALSE` once may still be tried on later packets if no dissector has claimed the conversation. Once recognition becomes sufficiently strong, `conversation_set_dissector()` is appropriate to avoid repeated probing.

**Rule:** perform minimum-length checks before every heuristic read, reject ambiguous/truncated traffic with `FALSE`, and keep the negative path exception-free. Do not treat one negative heuristic result as permanent conversation state.

**Rule:** only claim the conversation after positive evidence is strong enough; for protocols whose initial cleartext packets are more recognizable than later encrypted packets, use those initial messages as the recognition anchor rather than weakening the heuristic for encrypted traffic.

**Corroboration:** merged stable MR !8339 likewise adds an explicit structural test before allowing TP-Link Smart Home traffic on an unregistered/shared port and records conversation state only after recognition.

## Preserve stream-relative context across TLS

Merged master MR !8358, authored by John Thacker and merged by Anders Broman, changes TLS subdissector context from a bare application-handle pointer to a `tlsinfo` structure carrying the app handle, a TLS-stream-relative sequence number, and an end-of-stream indication derived from TCP FIN or TLS `close_notify`.

HTTP uses the sequence number to avoid repeatedly rescanning chunked data and uses end-of-stream state to support `DESEGMENT_UNTIL_FIN` over TLS the same way it does over direct TCP.

**Rule:** an intermediate transport/security dissector that exposes a logical byte stream to an application dissector must forward the stream-position and completion metadata required to preserve the application's normal reassembly semantics. Encryption should not erase sequencing/end-of-stream information that the child needs.

**Testing implication:** test desegmentation both directly over TCP and through TLS, including FIN/close-notify completion and long chunked streams.

**Authority:** extremely high; merged master work by John Thacker, accepted by Anders Broman.

## Persistent analysis state belongs to the semantic object it describes

Merged master MR !8317, authored by John Thacker, moves HTTP host, method, URI, full URI, and response code from mutable conversation-wide fields into the specific request/response object. The old conversation-level "current value" model worked during one forward pass but produced wrong results when users scrolled backward, jumped directly to packets, or filtered and redisected frames out of order.

**Rule:** values whose lifetime is one transaction/request-response pair belong on that transaction object, not in a mutable conversation-wide "latest value" slot.

**Rule:** conversation-wide state may keep ordering/correlation structures, but redissection of an earlier packet must retrieve the transaction object associated with that packet rather than inherit whichever transaction happened to be dissected most recently.

**Review implication:** test random packet navigation, filtering, second-pass dissection, and HTTP pipelining/multiple transactions rather than validating state only during sequential first-pass capture traversal.

**Authority:** very high; merged master state-model correction authored by John Thacker.

## Mutate ordered history on the first pass; consume it on revisits

Merged master/release MRs !8316/!8319/!8320 fix Fibre Channel SRT state by performing the LUN lookup on revisits after the first sequential pass has populated the mapping. Merged release MR !8328 similarly prevents TLS message-segment state from extending itself during a second pass; the relevant reassembly state is only meaningful while the ordered history is first being constructed.

**Rule:** use the first ordered pass to build persistent correlation/reassembly history and use later redissection to read that history. Do not let out-of-order revisits mutate "current end", "latest transaction", or similar monotonic state unless the algorithm is explicitly designed to be order-independent.

**Rule:** a visited frame means "this frame was dissected before", not "this particular state mutation is safe to repeat".

## Fuzzing should promote violated text contracts to hard failures

Merged master MR !8355, authored by João Valverde, makes the fuzz-test harness pass `--log-fatal-domains=UTF-8` by default, with an explicit `-U` opt-out. At the same time, the text-formatting helper's documented contract is clarified so that a helper specifically intended to sanitize potentially invalid packet UTF-8 does not itself report the presence of invalid input as a contract violation.

**Rule:** use fuzzing to turn internal text-contract diagnostics into fatal, reproducible failures so APIs that promise valid UTF-8 cannot silently leak invalid strings.

**Boundary rule:** do not flag invalid packet bytes merely for reaching an API whose defined purpose is to sanitize those bytes. Distinguish a violated internal UTF-8 contract from intentionally handling hostile wire data.

**Authority:** high; merged master tooling/API work authored by João Valverde.

## Debug and Release builds expose different defect classes

Merged master MR !8352, authored and merged by João Valverde, explicitly changes the GCC warnings CI build to `CMAKE_BUILD_TYPE=Release` because optimization can expose warnings that a non-optimized build does not, while Release also removes assertions/debug-only code and can reveal unused-variable paths.

**Rule:** treat optimized Release compilation and assertion-enabled behavioral testing as separate coverage dimensions. Do not assume one build configuration substitutes for the other.

**Testing implication:** maintain a Release-oriented warning/build job when optimization-specific diagnostics matter, while separately keeping assertion-enabled test coverage.

## Use dissector tables as extension points instead of hard-coded special cases

Merged master MR !8325 replaces a hard-coded GAEN BLE advertisement UUID check with a string-keyed dissector table for service UUIDs. The parent dissector tries the registered service-UUID subdissector and falls back to generic service-data rendering if no subdissector claims the entry.

**Rule:** when a protocol contains a semantic discriminator that can select independently extensible payload formats, prefer a dissector table keyed by that discriminator over a growing chain of hard-coded protocol-specific cases.

**Rule:** keep a generic fallback representation when no registered subdissector handles the key; extension points should improve specialization without making unknown values disappear.

## Portable integer formatting must follow the integer type, not the platform's long width

Closed MR !8312 attempted to repair 64-bit GIOP format warnings using GLib format macros, but it was explicitly closed in favor of merged MR !8311. The accepted change uses `PRId64` / `PRIu64` for fixed-width 64-bit values.

**Rule:** format fixed-width integer types with the corresponding fixed-width format macros rather than assuming `long` or `unsigned long` is 64 bits. This matters on platforms such as macOS/LP64 versus Windows/LLP64 and avoids warning-as-error build failures.

**Weighting note:** !8312 is negative/superseded evidence; !8311 is the accepted implementation.

## Additional corroborating observations

- !8354 reinforces explicit UTF-8 conversion at Qt boundaries instead of relying on implicit `char *` to `QVariant` conversion.
- !8350/!8351 reinforce using `ENC_APN_STR` instead of manually rewriting DNS/APN length-label bytes into dots, avoiding invalid UTF-8 and duplicated parser logic.
- !8348/!8349 reinforce validating/sanitizing packet-derived HTTP header values before inserting them into string-valued protocol-tree fields.
- !8346 reinforces representing a single wire character as `FT_CHAR` and using registered range/value metadata instead of custom formatting.
- !8336 demonstrates validating a `QRegularExpression` before invoking repeated model searches; transient invalid patterns are normal while a user is typing.
- !8321/!8323 show that minimum dependency versions should match the oldest supported platform baseline and that release documentation must be updated with the build requirement.
- !8333 is down-weighted: it was closed and explicitly notes that the change was later reverted elsewhere.
