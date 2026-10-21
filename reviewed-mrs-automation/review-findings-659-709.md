# Review findings: Wireshark MRs !659-!709

Reviewed exactly 50 valid MR records, descending from !709 through !659, at corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054`. The numeric span contains 51 numbers because `mr_701.json` is a zero-byte corpus artifact and cannot represent a reviewed MR. Outcomes are 47 merged and three closed/unmerged (!708, !704, !697).

## High-value findings

- **!669 — incremental Wiretap metadata propagation (very high authority).** Guy Harris authored and merged a Wiretap/editcap/tshark change that stops assuming all Interface Description Blocks are known at file-open time. New IDBs are fetched as input is read and added to the output when the destination format supports interface IDs. This is strong precedent that stream-interleaved metadata must be propagated incrementally rather than snapshotted once.
- **!677 — capability queries instead of format-name tests (very high authority).** Guy Harris replaced a pcapng-specific test with `wtap_uses_interface_ids(file_type)`. Code should ask the semantic capability it needs rather than name the format that first required the behavior.
- **!667 — semantic block-copy helper (very high authority).** Guy Harris added `wtap_block_make_copy()`, centralizing type-aware allocation plus block copying and making the returned object's independent ownership explicit.
- **!707 — keep parser sentinels outside valid data and preserve fast-path grammar.** The Ethernet-address fast path used bit 0x80 to detect an invalid nibble sentinel, accidentally rejecting legitimate octets 0x80-0xff. Peter Wu helped refine the accepted implementation so invalid input is represented outside the valid byte domain and so ':'/'-' separator choice remains consistent throughout one address. This is useful guidance for optimized parsers: the fast path must recognize the same valid grammar as the reference path.
- **!696 — default-enabled heuristics need several independent structural checks.** Peter Wu's DNS-over-UDP heuristic for non-standard ports checks message length, opcode, query/response-specific count constraints, and bounded section counts before claiming traffic. The MR explicitly discusses false-positive risk and possible strengthening.
- **!675 — Data fallback must not suppress protocol discovery.** Peter Wu caught that an earlier TLS fallback placement could prevent heuristics from running. The merged implementation invokes the Data dissector only after heuristic and port-based application dispatch have declined.
- **!704 — static-analysis findings should be traced to invalid precondition producers (closed/unmerged, review guidance only).** Martin Mathieson and Richard Sharpe rejected returning hash value 0 for a NULL key merely to placate a checker: zero is a valid hash, and the key-producing path should not hand an invalid key to the hash table. The proposed implementation did not merge, so only the review method is retained.
- **!685 — do not imply fuzz-crash causality without evidence.** Gerald Combs changed the fuzz diagnostic from labeling HEAD as “Git commit” to “Latest (but not necessarily the problem) commit.” A failure observed at current HEAD is not evidence that the latest commit introduced it.
- **!659 + !673 — public WSLua additions must survive generated-documentation parsing.** Review of the new `pinfo.p2p_dir` mutability exposed that the WSLua documentation generator's identifier grammar did not accept digits after the first underscore. Peter Wu identified the parser defect; !673 fixed the generator rather than documenting one symbol manually.
- **!660 — return already-decoded values when callers need them.** Anders Broman steered SMB2's file-attribute helper toward an optional out-parameter so the caller can use the value already parsed by the helper instead of fetching the wire field again.
- **!693 — maintainer edit/rebase access is part of submission readiness.** Anders Broman required the source branch setting that lets target-branch maintainers rebase or make minor edits.
- **!698 — new dissector review basics.** The contributor supplied a representative capture; Alexis La Goutte requested symbolic TCP/UDP constants instead of magic values and removal of template debris.
- **!694/!695 — zero-initialize aggregate state when not every member is explicitly assigned.** USB HID changes `wmem_new` to `wmem_new0` for a descriptor structure consumed after partial setup.
- **!690, !682, !681, !664 — registered type/access width must match the actual wire field.** These merged fixes independently correct field types, item lengths, and offsets.
- **!709 — superseded negative evidence.** Guy Harris's broad workaround removed a shared URL macro after one translation unit failed to compile it. Already-reviewed !719 identified the real cause as a missing header include in tfshark, and !721 reverted the broad workaround. Treat !709 as evidence to verify a translation unit's include/API contract before replacing a shared abstraction.

## Lower-information MRs

Straightforward protocol/spec updates, release mechanics, backports, UI wording, and typo fixes were reviewed for outcome and diff but did not justify separate durable conventions unless they corroborated an existing rule.

No SMPTE ST 291/VANC packet type was encountered.
