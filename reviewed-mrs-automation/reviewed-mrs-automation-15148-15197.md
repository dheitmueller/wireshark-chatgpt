# Wireshark MR review automation — 2026-09-20

Model: GPT-5.6 Sol

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

## Selection and tracking

Before selecting this batch, the already-reviewed set was reconciled against the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md`, the supplemental automation tracking, the per-run ledgers under `reviewed-mrs-automation/`, and the immediately preceding exact run ledger. The historical !17571-!17620 batch remains explicitly preserved and counted. Selection used exact MR membership rather than assuming numeric ranges were complete merely because some members appeared in a ledger.

The immediately preceding exact run reviewed through !15198. Neither `reviewed-mrs.md` nor the aggregate automation tracking contains !15197 or !15148, and there is no per-run ledger overlapping this candidate batch. All fifty corpus records !15197 through !15148 are present and valid. These are therefore the fifty highest-numbered previously unreviewed MRs at the corpus commit above.

## Exact MRs reviewed in this run

!15197, !15196, !15195, !15194, !15193, !15192, !15191, !15190, !15189, !15188

!15187, !15186, !15185, !15184, !15183, !15182, !15181, !15180, !15179, !15178

!15177, !15176, !15175, !15174, !15173, !15172, !15171, !15170, !15169, !15168

!15167, !15166, !15165, !15164, !15163, !15162, !15161, !15160, !15159, !15158

!15157, !15156, !15155, !15154, !15153, !15152, !15151, !15150, !15149, !15148

Count: **50 MRs**.

Status weighting: **50 merged, 0 closed/unmerged**. Master changes were used as the primary precedent where the same fix also appeared as a release backport; release-branch copies were treated as corroboration rather than independent rules.

## Durable findings promoted to the notebook

### Keep every tvbuff API call in one explicit coordinate system

Merged master !15194 fixes two Kafka Snappy regressions introduced by a helper refactor. Chunk offsets were local to a Kafka message beginning at a nonzero parent offset, while `tvb_child_uncompress_snappy()` was invoked on the parent tvbuff; the accepted fix adds the outer base exactly once. The same MR also restores checking for a NULL child tvbuff when decompression fails.

Extended `tvbuff-coordinate-conventions.md` with the complementary rule that parent-tvb operations need parent-relative offsets, plus explicit nullable-child handling.

### Treat recursion bounding as explicit Wireshark contributor policy

Merged master !15189 was authored by Gerald Combs and merged by Anders Broman. It adds developer documentation requiring recursive dissectors to ensure they cannot recurse excessively, directs unbounded cases to the dissection-depth helpers, and connects the policy to the clang-tidy recursion check. Because this is direct project documentation from Gerald, it receives unusually high authority.

Extended `parser-recursion-conventions.md` with this project-policy anchor.

### Reset transient queued work at capture replacement boundaries

Merged master !15187, authored by John Thacker and merged by Anders Broman, clears a queued-redissection state flag while opening a new capture. A fresh capture must not inherit deferred operation state that belonged to the previous file.

Extended `state-refresh-conventions.md` with a resource-lifecycle rule for queued/transient state.

### Check nullable pointer results before doing pointer arithmetic

Merged master !15167, authored and merged by John Thacker after a Coverity finding, fixes HTTP code that performed `ws_strnstr(...) + 3` before checking whether the search returned NULL. The NULL check must precede arithmetic; checking the adjusted pointer is already too late.

Extended `c-api-call-contract-conventions.md` with the nullable-result-before-transformation rule.

### Use model-wide notifications when the entire model changed, and measure library-side scaling

Merged master !15174, authored and merged by John Thacker, replaces an all-packets `dataChanged` notification that scaled linearly with millions of packet rows under Qt 6. The MR reports roughly 1 second at 1.4 million packets and 9 seconds at 12 million, versus about 5–8 ms using layout change notifications while preserving current/selected rows. Release-4.2 !15177 and release-4.0 !15178 carry the same fix.

Extended `ui-model-state-conventions.md` with a notification-semantics and scaling rule.

### Evolve UAT schemas additively and default missing fields for older rows

Merged master !15181, authored and merged by Martin Mathieson, includes direct John Thacker guidance on version-skew behavior for UATs. Older Wireshark versions can tolerate newer rows with unknown trailing fields with a warning; newer versions reading old rows should use `uat_set_default_values()` so newly appended fields have defined values.

Added `uat-schema-evolution-conventions.md`.

### Retap a stable live-capture snapshot and validate admission before resetting listeners

Merged master !15166, authored and merged by John Thacker, snapshots the current frame count before retapping a live capture so packets arriving during the traversal are not tapped once by the retap and again by normal live processing. The same change moves the `read_lock` failure check ahead of tap-listener reset, preventing a rejected nested traversal from leaving statistics partially cleared.

Added `live-capture-retap-conventions.md`.

## Strong corroborating evidence retained without duplicating existing rules

- !15196 is the master Time Shift Qt lifetime fix whose release backports were promoted in the preceding run: removing a nested event loop changed AutoConnection delivery to synchronous DirectConnection, so the accepted fix explicitly queues redissection delivery. It reinforces the existing Qt signal/lifetime convention rather than creating another one.
- !15197 fixes semantic skipping of zero-length BLF log containers and includes Guy Harris discussion about the dependency chain required for a safe release backport. Guy's review is useful evidence that a backport decision must account for prerequisite correctness fixes and not blindly cherry-pick an isolated commit.
- !15193 fixed clang-analyzer-reported uninitialized Kafka request arguments before merge, reinforcing the existing definite-initialization/static-analysis guidance.
- !15192 corrects legacy Kafka wire layout details: a pre-KIP-482 byte length is 32-bit, and two values share one flag octet so the offset must not advance between them. The author also validated Snappy decoding with an older real capture.
- !15188 supplied both normal and deliberately invalid NMEA captures and reported 500 `fuzz-test.sh` iterations. !15186 separately shows that a decompression-helper refactor benefited from obtaining a real Snappy capture rather than relying only on synthetic reasoning. Both strengthen existing capture-plus-fuzz testing guidance.
- !15183, authored and merged by John Thacker, changes a temporary storing `redissection_queued` from `gboolean` to the actual `rescan_type` enum. The old code happened to work because of representation details; this reinforces the existing rule that a variable's type should encode its semantic domain rather than a convenient boolean surrogate.
- !15182 converts RTCP RTPFB feedback dispatch from a switch to a dissector table and compares behavior with `randpkt`, reinforcing extension-point and behavior-preserving refactor guidance already present in the notebook.
- !15179 makes IO Graph tap removal track whether the listener is still registered, reinforcing idempotent/lifecycle-aware cleanup when a capture closes before a dialog is destroyed.
- !15173, authored and merged by John Thacker, restores HTTP NULL guards because a response can legitimately be present without the earlier request in a partial capture. This corroborates the existing `capture-start is not conversation-start` and optional-prerequisite-state guidance.
- !15164 raises the Lua minimum to 5.3; follow-up !15176 updates the WSLua and developer documentation after reviewers called out the stale version text. This is useful submission evidence that dependency-floor changes must audit user/developer documentation as well as the build system.
- !15162, !15152, and !15149 all preserve source-of-truth discipline for generated dissector data: generator/specification sources are changed and generated output is regenerated rather than hand-edited. !15162 and !15152 also move large static structures toward const/read-only storage.
- !15161, !15160, !15159, and !15158 are accepted I/O Graph/UI correctness fixes but are local enough that they do not justify separate notebook rules beyond the existing UI-state guidance.
- !15157 is authored and merged by Guy Harris but is only an HTTPS URL update. Reviewer/author authority raises confidence in substantive architectural feedback; it does not turn a trivial change into a durable coding convention.
- !15156 removes dead profile-model URL behavior after the column was repurposed, preventing display-filter contents from being treated as filesystem paths. !15155/!15154/!15165 are WSLua documentation/version-tag maintenance. !15153/!15151 are generated 3GPP dissector upgrades. !15150 expands the project comment explaining why nested Qt event loops are avoided. These were reviewed and retained as contextual evidence but did not add distinct rules.
- !15195 and !15191 are small protocol correctness/typo fixes; !15190 and !15184 continue standard C `bool`/const modernization; !15180 removes redundant warning text; !15175 and !15168 are protocol data additions; !15172/!15171/!15170/!15169 are automatic data updates; !15163 adds an MMS preference. These did not yield additional general conventions.
- !15148 initializes an HTTP variable that can otherwise be read after a zero-iteration loop, reinforcing existing definite-assignment guidance.

## Continuation

`mr_15147.json` exists in the same corpus commit and is a valid merged MR (`E1AP: upgrade dissector to v17.8.0`). The corpus is therefore not exhausted; the next run should continue selection from !15147 downward, still subtracting the exact reviewed set rather than assuming contiguous range coverage.
