# Wireshark MR automation review ledger — !16398–!16349

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook base commit before this run: `c0c9fd718c1e6783b97d690b9672afc9d9ba267b`

Selection method: enumerate the complete recursive Git tree for the corpus commit, consult `reviewed-mrs.md` and all available tracking under `reviewed-mrs-automation/`, preserve/count the historical !17571–!17620 batch, subtract the exact already-reviewed MR set from the corpus MR set, sort numerically descending, and take at most 50. No numeric range was assumed reviewed merely because neighboring MRs appeared in a ledger.

This run reviewed exactly 50 previously unreviewed MRs:

- !16398
- !16397
- !16396
- !16395
- !16394
- !16393
- !16392
- !16391
- !16390
- !16389
- !16388
- !16387
- !16386
- !16385
- !16384
- !16383
- !16382
- !16381
- !16380
- !16379
- !16378
- !16377
- !16376
- !16375
- !16374
- !16373
- !16372
- !16371
- !16370
- !16369
- !16368
- !16367
- !16366
- !16365
- !16364
- !16363
- !16362
- !16361
- !16360
- !16359
- !16358
- !16357
- !16356
- !16355
- !16354
- !16353
- !16352
- !16351
- !16350
- !16349

## Review weighting and durable findings

Merged master MRs were weighted most heavily. Stable-branch backports were mainly corroborating evidence. Closed drafts and abandoned changes were reviewed for discussion value but not treated as accepted implementation precedent.

- **!16392 — merged master; very strong CLI contract guidance.** John Thacker added `editcap --extract-secrets`. Guy Harris challenged the initial plan to silently ignore inapplicable options and argued for warning or failure so users would not reasonably interpret ignored options as bugs. The accepted implementation rejects incompatible editing options with `WS_EXIT_INVALID_OPTION`, documents the compatibility set, and adds tests. Promoted to new `cli-option-interaction-conventions.md`.
- **!16393 — merged master; very strong allocation/container review.** During the Wiretap file-opening deduplication, Coverity/John Thacker identified a pointer-indirection allocation-size issue. Guy Harris explained that the code only happened to work because the relevant pointer types have equal size on supported platforms, recommended a typed allocator such as `g_new()`, and suggested `GPtrArray` for the variable-sized array of pointers. The author acknowledged the stale pointer-depth assumption and implemented the `GPtrArray` direction in merged follow-up !16407. Promoted to `memory-management-conventions.md`.
- **!16372 versus !16371 — accepted name-resolution state semantics.** John Thacker's merged !16372 records a failed EUI-48 resolution attempt only when resolution was actually requested, preventing repeated expensive misses without suppressing a future lookup merely because name resolution had previously been disabled. The competing !16371 draft used a broader tried-or-resolved mask; John called out the configuration-state problem and the draft was closed. Promoted to `name-resolution-conventions.md`.
- **!16350 — merged master; cache-lifetime architecture.** Signal PDU caches expensive value-name lookups, but one cache contains references into the other, so the two caches must be invalidated and rebuilt together. Promoted to `state-refresh-conventions.md` as a coupled-cache invalidation rule.
- **!16370 — merged master; generated-output validation corroboration.** Python/ruff cleanups deliberately regenerated all ASN dissectors and verified that generated output did not change. This strongly corroborates the existing notebook principle that generator/tooling refactors should validate generated artifacts, so no duplicate rule was added.
- **!16378 — merged master; Kerberos conversation/statistics cleanup.** The author discovered that an old `find_conversation`/`conversation_new` pattern did not behave correctly for UDP and switched to `find_or_create_conversation`; Anders Broman also required C99 boolean/type spelling in generated-template-related code. Useful corroboration of existing conversation and C99 conventions, not promoted separately.
- **!16355 and !16363 — merged master plus release-4.2 packaging backport.** The master RPM nghttp3 addition prompted Gerald Combs to call for matching `tools/rpm-setup.sh` support and John Thacker to question distro package availability; the later release-4.2 backport was explicitly challenged as to whether it belonged on that branch before ultimately merging. Useful packaging/backport context, but not enough for a new general rule.
- **!16353 — closed/unmerged and explicitly disavowed by its author.** The proposed c-ares initialization change was questioned by John Thacker against the library's init/cleanup contract; the author later stated the change was wrong and closed it. It was down-weighted and not used as implementation precedent.

Notebook convention commits made during this run before this ledger:

- `62f52a98ea7d767fa2249844f6f25612359bd5fd` — add CLI incompatible-option guidance.
- `be9737899103ced452664c94b83ef94842652508` — add allocation element-type / pointer-container guidance.
- `928438d60006027b379583c316ecd7d90f3a5865` — add negative name-resolution cache semantics.
- `aeb405051390dba40db68c5d0052984e4e44939c` — add coupled-cache invalidation guidance.
