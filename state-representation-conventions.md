# Wireshark State and Representation Conventions

This file records durable conventions for representing type/state domains and keyed state, extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Represent mutually exclusive type and orthogonal attributes separately

When a property has exactly one value from a closed set, model that property as an enum (or another single-valued type). Do not encode it as a bitmap merely because a second, orthogonal attribute was historically ORed into the same integer. Mixing the two domains makes direct equality unreliable, forces callers to mask bits before every switch, and makes it easy for some call sites to forget the masking entirely.

Merged master MR !20141 replaces the `PREF_*` bitmap-style preference type values with a dedicated enum and moves `PREF_OBSOLETE` into a separate Boolean `obsolete` member. Jaap Keuter's rationale is explicit: preference types are mutually exclusive, while obsolescence is independent of type. The accepted change removes repeated bit manipulation, makes direct type comparisons correct, and aligns Lua and epan preference representations. John Thacker merged the result after review.

**Implementation rule:** model each independent semantic dimension independently. Use an enum for mutually exclusive variants and separate fields/flags for orthogonal attributes. A bitmask is appropriate when multiple values from the same domain may legitimately coexist; it should not be used to smuggle unrelated state into a nominal type discriminator.

**Confidence:** Very high. Merged master refactor with the semantic mismatch and resulting failure modes documented directly in the MR.

## Keep hash-key representation consistent with the hash and equality contract

A map's key representation is part of its semantic contract. Keys inserted through one representation must be looked up and inserted through that same representation unless the hash/equality functions explicitly normalize across representations. Two keys that denote the same conceptual integer are not interchangeable if one is an allocated integer object and the other is an integer encoded directly in a pointer.

Merged master MR !20161 fixes `tap-wspstat`, where the hash was initially populated with GLib-allocated integer keys but misses were later inserted with `GINT_TO_POINTER()`. The mismatch produced observable lookup failures: the sample `wap_google.pcap` reported status names as `(null)` until initialization was changed to use the same `GINT_TO_POINTER()` representation. The same MR also registers a tap finish callback and uses `g_hash_table_new_full()` so key/value cleanup is owned by the container lifecycle.

**Implementation rule:** choose a key representation once and make creation, lookup, insertion, hashing, equality, and destruction agree with it. When using GLib pointer-encoded integer keys, do not mix them with pointers to separately allocated integer objects. Prefer container constructors/destructors that encode ownership explicitly instead of maintaining a parallel ad-hoc free walk.

**Confidence:** High. Merged master correctness fix with a concrete sample-capture behavior change and maintainer approval.

## Store sparse state transitions instead of per-frame copies when state changes infrequently

State that is constant across long runs of frames should normally be represented by the points where it changes, not by allocating a copy for every frame in the capture. This both matches the semantics and avoids memory proportional to unrelated traffic.

During the extensive review of merged SSH MR !20158, John Thacker explicitly rejected an array indexed across every frame as wasteful. He pointed to `p_add_proto_data()` for truly per-frame data and to a `wmem_tree_t` keyed by frame number for state transitions such as REKEY events. With `wmem_tree_lookup32_le()`, dissection can retrieve the most recent transition at or before the current frame, which naturally models state that remains valid until the next transition. His review also corrected the protocol model itself: SSH sequence numbers are not reset by ordinary rekeying; reset behavior is conditional on strict KEX semantics.

**Implementation rule:** first identify whether information is per-frame or transition-based. Use per-frame proto data only when every relevant frame genuinely owns distinct information. For epochs, key changes, negotiated modes, or similar state that changes at discrete frames, store the transition points in file/conversation-scoped state and perform predecessor lookup during redissection. Do not allocate capture-wide arrays simply because frame numbers provide a convenient index.

**Review/testing implication:** stateful protocol changes should be exercised across multiple simultaneous sessions and multiple state transitions, and both first-pass/live and later redissection/offline paths should be checked. !20158 supplied multi-session captures, forced REKEY traffic, key logs, debug output, and both live and second-pass TShark results.

**Confidence:** Very high. Merged master change with extensive direct John Thacker review and concrete multi-session/rekey validation artifacts.

## Prefer simplification that exposes common structure before deduplicating parallel readers

When several readers implement the same simple record model through different parser-generator or local-structure machinery, simplifying the individual implementation can be a legitimate preparatory step toward shared infrastructure—provided the grammar is simple enough that the replacement remains clear.

Merged !20146 replaces the relatively small candump Flex/Lemon parser with straightforward C specifically to make commonality with the other text-based CAN wiretap readers easier to identify and refactor. Merged !20152 then moves those readers toward shared `socketcan.[ch]` structures and the common `wtap_socketcan_gen_packet()` record-construction path. Taken together, the sequence shows a useful refactoring order: reduce accidental implementation differences first, then extract the true shared representation/helper.

**Architecture rule:** do not preserve abstraction machinery merely because it already exists when that machinery obscures a small grammar and blocks convergence with sibling implementations. Conversely, simplification should be justified by the actual grammar and a concrete reuse direction; it is not a blanket preference for hand-written parsers over parser generators.

**Confidence:** High. Two merged master changes forming an explicit refactoring sequence, although the MRs contain little substantive reviewer discussion.

## Give captures that start midstream one coherent fallback state model

Stateful protocol dissectors cannot assume that a capture contains the greeting, login, negotiation, or setup messages that normally establish feature flags. When those packets are missing, fall back from one centrally defined, conservative state model rather than sprinkling independent guesses through individual parse branches.

Closed MR !15738 tried to improve MySQL/MariaDB mid-conversation captures by assuming one capability flag (`MYSQL_CAPS_DE`) when the login packet was absent. John Thacker questioned whether the same assumption needed to apply at several other capability checks and explored whether packet structure could resolve the ambiguity instead. The proposal was then superseded by merged master MR !15741, which establishes a minimal default set of capability flags for conversations whose greeting/login was not captured, while acknowledging that genuinely ambiguous features can still require limited inference.

**Implementation rule:** initialize missing negotiated state once at the conversation/state boundary with the smallest coherent baseline that permits useful dissection, and then refine that state when authoritative packets are observed. Do not make the same missing-handshake condition mean different things in different parser branches.

**Review rule:** test stateful dissectors with captures that begin after setup as well as with complete sessions. When two wire interpretations remain indistinguishable without the omitted negotiation, document the ambiguity and prefer a conservative project-wide default over ad-hoc branch-local guesses.

**Confidence:** Very high. The weaker single-flag proposal was explicitly superseded after substantive John Thacker review by a merged alternative that centralized the fallback state.

## Choose invalid-state sentinels outside the valid value domain

A sentinel used for “unknown”, “not present”, or “invalid” state must not collide with a value that the protocol or application can legitimately produce. Reusing a familiar zero value as a stand-in for absence is unsafe when zero itself has valid semantics.

Merged master MR !15667 changes SMB2 request/response frame bookkeeping to use `UINT32_MAX` for invalid `frame_req` / `frame_res` state because frame number 0 can be valid in the surrounding logic. The change was part of the accepted SMB2/Kerberos decryption series merged by Anders Broman.

**Implementation rule:** define absence/error sentinels from the semantic domain, not by habit. Before choosing 0, -1, a maximum integer, or another magic value, verify that the chosen representation is impossible for every valid producer and preserved correctly by conversions and storage types. Prefer an explicit validity flag or richer type when the value domain leaves no unambiguous sentinel.

**Review rule:** when a state field doubles as both data and validity marker, audit all producers and comparisons for legitimate boundary values. A sentinel collision can make valid state appear missing and can be especially hard to diagnose when only captures near the boundary exercise it.

**Confidence:** High. Merged master correctness change in a multi-commit protocol series; the commit message explicitly records the reason for abandoning zero as the invalid marker.