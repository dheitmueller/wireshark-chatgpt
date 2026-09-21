# Wireshark MR review automation: !14899–!14948

Corpus repository commit: `1c7ad509887ee25079a7865cc62ba18cba06f49a`

Notebook baseline before this run: `a68907f74bb3550fbf35d644c62c7a81eeda506c`

Selection method: rebuilt the already-reviewed set from the available review tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, aggregate automation tracking, and the per-run files under `reviewed-mrs-automation/`. The historical !17571–!17620 batch was explicitly preserved and counted. The immediately preceding exact ledger records !14998 through !14949. Selection was by individual MR membership rather than assuming numeric ranges were complete.

The 50 highest-numbered corpus MRs not already in the reviewed set were !14948 through !14899 inclusive. This run reviewed exactly those 50. All 50 are merged; there are no closed/unmerged MRs in this batch.

## Exact reviewed MR set

!14948, !14947, !14946, !14945, !14944, !14943, !14942, !14941, !14940, !14939,
!14938, !14937, !14936, !14935, !14934, !14933, !14932, !14931, !14930, !14929,
!14928, !14927, !14926, !14925, !14924, !14923, !14922, !14921, !14920, !14919,
!14918, !14917, !14916, !14915, !14914, !14913, !14912, !14911, !14910, !14909,
!14908, !14907, !14906, !14905, !14904, !14903, !14902, !14901, !14900, !14899.

Count: **50**.

## Durable notebook findings promoted

- **!14929 and !14926 — merged master; extremely high-authority Guy Harris evidence.** Guy authored and merged both changes. !14929 introduces `PI_DISSECTOR_BUG` so `DissectorError` is no longer reported as malformed packet data: an internal dissector invariant failure is a Wireshark bug, not evidence against the packet. !14926 moves `ei_unreassembled` under the unreassembled protocol rather than the malformed protocol; !14927 and !14928 are stable backports. Promoted to `protocol-input-diagnostics-conventions.md`: diagnostic ownership/group must identify the semantic source of failure rather than blaming packet malformation for a dissector bug or missing reassembly. Notebook commit: `a9232b93772c6b9267c53951d13ae790e2663d5b`.

- **!14923 — merged master.** The World of Warcraft dissector conversion moves protocol-version state from a global into per-conversation state. Its test capture contains all supported protocol versions sequentially and exposed the prior global-state bug. Promoted to `dissector-state-conventions.md`: negotiated version/mode belongs to the session that negotiated it, and version-sensitive tests should include multiple sequential or concurrent session versions. Notebook commit: `e8966ff8e9ef69c42f4d5b04fefa8c276424e8f2`.

- **!14901 — merged master, authored by John Thacker and merged by Gerald Combs.** TFTP Export Objects failed in one-pass TShark because early blocks were discarded until the final block had established that the transfer was complete, a fact unavailable to a streaming first pass. The accepted code preserves blocks during the first pass in file-scope memory and uses the known final size on revisits. Stable !14908–!14910 carry the same fix. Promoted to new `single-pass-analysis-conventions.md`: one-pass consumers must preserve the state needed for results before future-dependent predicates become knowable; test both streaming and revisit/two-pass execution models. Notebook commit: `b9b8f372b63c1747731865c8af65fd183d0cdb42`.

## Strong corroborating / historical evidence retained

- **!14911 — merged, authored by John Thacker.** The Wiretap C99 conversion gives especially clear type-contract evidence: `bool` and `gboolean` are not pointer- or callback-signature-compatible merely because their scalar values convert; dependency callbacks must retain dependency-declared types, while Wireshark-owned boolean contracts should use `bool`. `can_write_encap` deliberately remains integer-valued because it returns error codes rather than a truth value. This directly corroborates the existing `c-type-conventions.md` rules, so no duplicate prose was added.

- **!14946 — merged SocketCAN classification fix with substantive Guy Harris discussion.** Guy emphasized that captures may be cropped and that multiple Wiretap readers normalize different source formats to `WTAP_ENCAP_SOCKETCAN`, so the dissector cannot blindly require one canonical padded record size. The discussion is useful historical evidence about distinguishing semantic indicators from producer-specific layout details. Later reviewed SocketCAN work (!18710/!18831 and the subsequently accepted design) supersedes this particular stage of the heuristic evolution, so !14946 was not promoted as a standalone current rule.

- **!14902 plus !14905–!14907, !14930–!14943, !14945** form a broad recursion-hardening series. Recursive dissectors increment/decrement Wireshark's dissection-depth guard around recursion, while known structurally bounded recursion receives explicit clang-tidy suppression/comments. These MRs strongly corroborate the notebook's existing bounded-recursion and stack-like depth-state rules rather than requiring another convention.

- **!14913** explicitly treats the World of Warcraft message definitions as generated/source-authoritative data: a spelling cleanup had to be undone because the misspellings are what the game/source generator defines, and Martin Mathieson noted the file should be treated as generated. This corroborates the existing generated-code/source-of-truth convention.

- **!14922** adds NMEA ZDA dissection with valid and invalid capture samples and reports 1000 fuzz iterations. Review caught a registered-field length/type warning and a dead store before merge. This is good corroboration for focused valid/malformed samples, fuzzing, and checker cleanliness on dissector additions.

- **!14914 plus !14918–!14920** validate the USB class-data discriminator before casting shared class data to the video-specific structure. This corroborates existing nullability/type-contract guidance for shared tagged state.

- **!14921** checks remaining NMEA bytes before reading a CR/LF terminator; **!14899** treats trailing CIP Forward Open response fields as optional based on remaining bytes. Both corroborate existing parser-boundary/optional-field guidance.

- **!14900**, authored by John Thacker, fixes BCD filler semantics so `0xf` terminates in either nibble (and multiple filler nibbles are handled), including big-endian masking. This is a strong protocol/helper correctness fix but too representation-specific to justify a new cross-cutting rule.

- **!14912** is a substantial MSVC build-system compatibility fix for ASAN versus `/Qspectre`, preserving Spectre mitigation where the available library layout supports it and restricting ASAN to build types with usable debug information. It is valuable platform-specific build history, but not sufficiently general to add another notebook convention.

## Lower-signal / maintenance material

The remaining merged MRs were inspected for discussion and diff significance. They include dependency updates, release-branch backports of the master fixes above, straightforward protocol flag/source corrections, spelling/tooling maintenance, and narrow generated-definition updates. Stable backports were weighted as confirmation of accepted behavior rather than independent architecture decisions.

## Corpus frontier

`mr_14898.json` is present and is a valid merged MR at the same corpus commit, so the corpus is not exhausted. The next run should rebuild the reviewed set and continue from the highest-numbered unreviewed MR; absent newly scraped higher-numbered material, the descending frontier is !14898.
