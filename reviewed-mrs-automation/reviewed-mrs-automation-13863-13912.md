# Wireshark MR review run: !13912 through !13863

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `fdca86c0ebcff519345d8412375afc3822888d94`

Selection was reconciled against the available review tracking in `reviewed-mrs.md`, the aggregate automation ledger, and the per-run ledger inventory under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains part of the already-reviewed set. The preceding exact ledger ended at !13913 and had explicitly treated !13912 only as a frontier probe. The corpus had not advanced, so exact set subtraction selected the fifty highest-numbered previously unreviewed corpus MRs listed below.

## Exact reviewed set

!13912, !13911, !13910, !13909, !13908, !13907, !13906, !13905, !13904, !13903,
!13902, !13901, !13900, !13899, !13898, !13897, !13896, !13895, !13894, !13893,
!13892, !13891, !13890, !13889, !13888, !13887, !13886, !13885, !13884, !13883,
!13882, !13881, !13880, !13879, !13878, !13877, !13876, !13875, !13874, !13873,
!13872, !13871, !13870, !13869, !13868, !13867, !13866, !13865, !13864, !13863.

Exactly 50 MRs were reviewed. Status mix: 48 merged; two closed/unmerged (!13906 and !13874). !13906 was superseded by merged !13907, and !13874 was an empty/duplicate packaging change superseded by merged !13875, so both closed MRs were given substantially less architectural weight.

## Durable findings promoted

- **!13910, with stable backports !13911 and !13912 — integer-compatible API parameters still have distinct semantic domains.** John Thacker's merged master fix replaces an RSVP runtime `type` value that had accidentally been supplied as the `proto_tree_add_item()` encoding argument with `ENC_BIG_ENDIAN`. The lesson was added to `c-api-call-contract-conventions.md`: compiler-compatible integer arguments are not semantically interchangeable, and callers must validate each argument against the API's documented domain.

- **!13879, with stable backports !13880 and !13881 — hash/equality callbacks must match the concrete key storage type.** A table whose keys are `gint` used `g_int64_hash()`/`g_int64_equal()`, causing the callbacks to read beyond the actual 32-bit key object and exposing an uninitialized read. The notebook now records that GLib hash helpers describe the pointed-to memory representation, not merely the conceptual numeric range of the identifier.

- **!13900 together with split-out !13910/!13911/!13912 — independently backportable fixes should be separated from feature work.** During review of the merged RSVP feature MR, Alexis La Goutte called out the unrelated encoding-argument correction and asked whether it should be a specific commit so it could be backported; John Thacker agreed. The correction then landed separately on master and both maintained stable branches. This was added to `stable-branch-submission-conventions.md`.

- **!13870 — reproduce memory-safety fuzz failures with the relevant sanitizer/allocator environment.** Martin Mathieson could not readily reproduce the E2AP fuzz failure in an ordinary setup. Gerald Combs explained that he generally needs ASAN plus `WIRESHARK_DEBUG_WMEM_OVERRIDE=strict` and `G_SLICE=debug-blocks` to replicate the fuzz environment sufficiently; John Thacker confirmed ASAN reproduction and the fix. This was added to `fuzz-harness-conventions.md` as a complement to the existing exact-target reproduction rule.

## Strong corroborating findings retained without duplicating notebook rules

- **!13865 — exception-safe cleanup for non-wmem resources.** John Thacker's merged COSE fix uses `CLEANUP_PUSH`/`CLEANUP_CALL_AND_POP` to release `GVariant` references even when dissection throws, and unreferences an existing principal before replacement. This strongly corroborates the existing allocator/lifetime rule that normal-return cleanup is insufficient on exception-capable dissection paths.
- **!13886 — do not pretend unsupported data was dissected.** The accepted SSH change removes branches that recognized signature families but displayed none of their content, allowing the bytes to fall through to generic data instead of being silently hidden.
- **!13883/!13882 — prefer a verified full checksum match over accepting a partial-checksum coincidence.** The accepted TCP change requires a nonzero computed partial checksum before classifying a packet as partial-checksum-valid.
- **!13878/!13877 and !13876 — fully initialize state structs before hashing or later use.** The T.38 and SCTP fixes initialize newly relevant fields whose uninitialized values could influence state handling.
- **!13869/!13868/!13867 — ASN.1 recursion protections were backported across stable branches.** These are accepted backports of the recursion-depth hardening; the corresponding master change lies below this run's frontier and can be weighted more heavily when it is reviewed directly.
- **!13864 — respect TVB helper preconditions even for protocol-valid zero lengths.** `tvb_bytes_to_str()` asserts on a zero length, so the MLE dissector now avoids the conversion while still advancing by the TLV's actual length.
- **!13863 — child-process file-descriptor inheritance can extend pipe lifetime and hang shutdown.** The release-4.2 backport sets `FD_CLOEXEC` on the mmdbresolve pipe descriptors so subsequently exec'd dumpcap processes do not keep them open. The corresponding master MR !13862 was fetched only as the next-frontier check and is deliberately not counted as reviewed here; any new notebook rule should prefer that master evidence in the next run.

The remaining MRs were release preparation/versioning, generated release-note automation, packaging/backports, file-association updates, protocol fixes, or direct stable backports that either reinforced existing conventions or did not provide a stronger durable rule than the items above. Merged changes were weighted more heavily than superseded/closed alternatives.

## Notebook commits from this run

- `e575849cd254f29a38f781efc2856065ee393e2d` — document C API semantic-domain and hash-width contracts.
- `0902b68c843460f0dd7394de0709edada575353c` — document fuzz-environment reproduction requirements.
- `da041c6c68c05652ecb8f1bcd74ccb72dabafcfc` — document split-out backportable fix workflow.

## Frontier

`mr_13862.json` exists in corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054`. It was fetched only to verify that the corpus continues and was **not** counted as reviewed. Therefore the corpus is not exhausted; absent newly scraped higher-numbered unreviewed material, !13862 is the next descending candidate.
