# Wireshark MR review automation: !21241-!21290

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to older
Exact MRs reviewed in this run: 50

Before selecting this batch, the per-run ledgers under `reviewed-mrs-automation/` and the sparse entries in `reviewed-mrs.md` were consulted and combined as the already-reviewed set. Selection was by individual MR-number membership, not by assuming that any numeric range was complete. The immediately preceding exact ledger covers !21291-!21340, and no existing tracking entry marked an MR in !21241-!21290 as already reviewed. The historical !17571-!17620 batch remains preserved and counted. The exact set below is therefore the 50 highest-numbered corpus MRs at the recorded corpus commit that had not already been reviewed.

Weighting policy: merged master changes and substantive maintainer feedback carry the most weight; release backports primarily corroborate the corresponding accepted behavior; open drafts, closed work, reverts, and superseded iterations are deliberately down-weighted. Highly authoritative maintainer evidence, especially Guy Harris's correctness and API guidance, receives correspondingly high weight.

## Exact reviewed set

- !21290
- !21289
- !21288
- !21287
- !21286
- !21285
- !21284
- !21283
- !21282
- !21281
- !21280
- !21279
- !21278
- !21277
- !21276
- !21275
- !21274
- !21273
- !21272
- !21271
- !21270
- !21269
- !21268
- !21267
- !21266
- !21265
- !21264
- !21263
- !21262
- !21261
- !21260
- !21259
- !21258
- !21257
- !21256
- !21255
- !21254
- !21253
- !21252
- !21251
- !21250
- !21249
- !21248
- !21247
- !21246
- !21245
- !21244
- !21243
- !21242
- !21241

## Review highlights and weighting notes

- **!21282 — deep / promoted, merged release-4.6, extremely high confidence.** Guy Harris-authored/merged C12.22 fix avoids calling `memcpy()` with a NULL source even when the copy length is zero. UBSan exposed that the API's non-null pointer contract still applies independently of the zero byte count. Promoted to `c-api-call-contract-conventions.md`.
- **!21244 — deep / promoted, merged release-4.6.** Pcapng hardening fixes integer-overflow and error-path leaks, but its most durable API lesson is ownership: `wtap_block_add_string_option_owned()` transfers the string only on success, so callers must check the result and free on failure. The MR explicitly argues that such an ownership-bearing result should be must-use. Promoted to `c-api-call-contract-conventions.md`.
- **!21254 — deep / promoted, merged release-4.6.** John Thacker separates release-4.6 fuzz and Valgrind-fuzz resource groups from master because sharing the serialization keys caused two daily branch fuzz schedules to take turns and fall behind. Gerald Combs approved and merged it. Promoted to `ci-resource-scheduling-conventions.md`.
- **!21288 — deep / strong corroboration, merged release-4.6, Guy Harris-authored/merged.** NetMon validates a frame-table length and offset against the actual file size before allocating/reading it, preventing a malformed file from requesting an arbitrarily large allocation. Reinforces existing untrusted-length/allocation guidance.
- **!21279 — deep / strong corroboration, merged release-4.6, Guy Harris-authored/merged.** LOG3GPP replaces several huge process-global static parser buffers and other mutable globals with per-parser private state plus a small probe buffer, reducing `.bss` and removing conflict if multiple parser instances run concurrently. Reinforces the existing reentrancy/narrowest-lifetime state rule.
- **!21273 — strong corroboration, merged.** K12 gives its source-description hash a value destroy function so replacing duplicate keys releases displaced owned values. Reinforces container ownership/destructor guidance.
- **!21268 — deep / corroboration, merged master with Guy Harris approval/merge.** Coverity found that Darwin generated metadata used the trace-tag field/value when displaying packet generation count. The accepted one-line correction keeps registered field identity and extracted semantic value aligned; reinforces static-analysis and protocol-field semantic-consistency guidance.
- **!21262 — strong public-header corroboration, merged release-4.4.** An installed public header had begun including `packet-ipv6.h`, but `packet-ipv6.h` itself was absent from `DISSECTOR_PUBLIC_HEADERS`. The fix adds it to the installed public set, reinforcing header dependency closure and clean-consumer validation.
- **!21285, !21283, !21274, !21251, and !21242 — memory-management corroboration.** Merged leak fixes cover fuzz-discovered temporary allocations and error paths in AUTOSAR DLT, TTL, and pcapng. They reinforce existing cleanup-on-all-exits guidance without adding a distinct rule.
- **!21287 — merged Guy Harris tooling cleanup.** License detection avoids producing duplicate `ISC ISC` classification when both SPDX identification and matching license text are present. Useful provenance-tooling evidence, but existing licensing/provenance conventions already cover the broader lesson.
- **!21284 — merged external-library import.** The c128 import is useful provenance/dependency evidence but does not add a distinct rule beyond the existing contribution-licensing notebook guidance.
- **!21276 and !21272 — merged reverts / deliberately down-weighted.** These revert intermediate LDA Neo trailer renaming attempts; the later corrected naming series is the stronger final evidence, so the reverted implementations are not treated as precedent.
- **!21266 — open draft / deliberately down-weighted.** Michael Mann preferred replacing GUI-specific “selected packets” handling with marked-frame state for architectural cleanliness, but John Thacker explicitly questioned the loss of per-packet interpretation flexibility. Because the snapshot remains draft/open and the functionality tradeoff is unresolved, the discussion is retained only as provisional design caution.
- **!21256 and !21255 — closed / deliberately down-weighted.** The version bump and earlier CI resource-group attempt were not merged; !21254 supplies the accepted CI outcome.

The remaining MRs in this exact set were reviewed for corpus state, diff/relevant implementation, discussion/maintainer input, and relationship to already-recorded conventions. They were predominantly documentation moves, typo/const cleanup, stable backports, protocol-specific field corrections, UI redraw behavior, or intermediate/final pieces of the LDA naming sequence and did not justify additional durable notebook rules.