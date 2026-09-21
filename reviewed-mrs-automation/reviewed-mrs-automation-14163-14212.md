# Reviewed MRs automation: !14163–!14212

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook repository: `dheitmueller/wireshark-chatgpt`
- Notebook commit at start of run: `3c95483b6dd044be7d6f6baceaf3ecb92e304c49`
- Review direction: descending MR number from the newest available previously-unreviewed MR.
- Selection method: rebuilt the already-reviewed MR-number set from `reviewed-mrs.md` and the available per-run files under `reviewed-mrs-automation/`, preserving/counting the historical !17571–!17620 batch. The corpus had not advanced since the immediately preceding exact !14213–!14262 run, so exact set subtraction selected the 50 MRs below. No numeric range was assumed reviewed merely from partial ledger coverage.

## Exact MRs reviewed in this run

!14212, !14211, !14210, !14209, !14208, !14207, !14206, !14205, !14204, !14203,
!14202, !14201, !14200, !14199, !14198, !14197, !14196, !14195, !14194, !14193,
!14192, !14191, !14190, !14189, !14188, !14187, !14186, !14185, !14184, !14183,
!14182, !14181, !14180, !14179, !14178, !14177, !14176, !14175, !14174, !14173,
!14172, !14171, !14170, !14169, !14168, !14167, !14166, !14165, !14164, !14163.

Exactly 50 MRs were reviewed.

## Status weighting

- 48 merged MRs.
- 2 closed/unmerged MRs: !14187 and !14168.
- !14187 was down-weighted because it was superseded by the merged !14188 implementation of the same RTP/Opus work.
- !14168 was down-weighted because it was abandoned as a submission/workflow attempt after maintainer guidance to use a separate source branch and permit maintainer edits; its process discussion is useful but it is not accepted implementation evidence.

## Durable notebook updates

- **!14203 — merged master, John Thacker:** promoted to `protocol-tree-hierarchy-conventions.md`. A tree optimization must not represent a logically distinct mutable child by reusing the parent's node when length/representation/identity can subsequently be observed or changed. Protocol-hierarchy statistics were a concrete downstream consumer of the final protocol-item lengths.
- **!14194 and !14178 — merged master, strong John Thacker evidence:** promoted to `memory-management-conventions.md`. Data retained by a statistics/UI object must live for the consumer's lifetime, not merely packet or capture-file lifetime. A statistics window may exist without a capture and can outlive file close, so packet/file-scope allocations are not automatically safe for retained maps or labels.
- **!14170 — merged master, approved by Martin Mathieson:** promoted to `dissector-context-flow-conventions.md`. A negative diagnostic whose truth depends on the absence of a later correlated packet must be delayed until a full forward analysis pass has had a chance to discover that counterpart. This is a specific exception to the general rule that already-known visible expert information should not be suppressed on redissection.
- **!14204 — merged master, John Thacker:** added `generated-source-syntax-conventions.md`. Arbitrary packet text embedded in source-code exports must be delimited/escaped so it cannot alter the generated language's lexical or preprocessing structure. The motivating C-array export bug was a trailing backslash in a `//` comment causing preprocessor line splicing.

## Strong corroborating evidence retained without duplicate notebook rules

- **!14177 — merged master, substantial Guy Harris review:** gave high authority to Guy's distinction between expert-info category and severity and to his reasoning about receive/link-layer versus adapter-originated errors. This independently corroborates the already-recorded `expert-info-taxonomy-conventions.md` rule from the later !14255 work, so no duplicate rule was added.
- **!14181 — merged master, John Thacker:** display-filter macro/field disambiguation must choose the earliest relevant delimiter within the construct; independent whole-remainder searches can let a later `#` override an earlier closing brace. This reinforces the existing token-boundary/nesting convention from !14361.
- **!14179 — merged master, John Thacker:** MATE ranges demonstrate that numeric offsets are meaningful only together with their backing TVB/data-source identity. This reinforces the existing data-source rule in `proto-tree-range-conventions.md` and connects directly to !14203's tree-visibility fix.
- **!14196 (master) / !14198 (stable backport) — John Thacker:** prefer bounded TVB string extraction when a length is known rather than a raw helper that assumes the caller has already proved the range; malformed/truncated packets must take the normal bounds-exception path instead of reading uninitialized/out-of-range data.
- **!14197 — merged master, John Thacker:** exception paths through malformed XMPP input still need deterministic container cleanup; reinforces exception-safe ownership guidance.
- **!14190, !14191, !14192, !14206, !14208 — merged checker/registration cleanups, mostly Martin Mathieson:** reinforce that field-registration invariants such as maskless `FT_BOOLEAN` using `BASE_NONE`, string/IP display bases, and NULL versus literal-empty metadata are suitable for executable checker enforcement rather than review folklore.
- **!14167 — merged SSH extension support:** Alexis La Goutte asked for a capture; the contributor supplied both a sample pcapng and key-log material covering the encrypted extension messages. This reinforces the established expectation for concrete sample captures with protocol/dissector feature additions.
- **!14171 — merged master, Gerald Combs:** setup scripts should fail promptly on command errors, and when they rely on a specific CLI option they must verify/install a tool version that actually supports that option. Retained as build/tooling corroboration rather than creating a narrow macOS-only rule.
- **!14163 — merged master, John Thacker:** imported/merged temporary captures must be marked as tempfiles so save/close/recent-file behavior is correct, and synthetic temporary paths must not overwrite the user's last real open-directory state. Useful UI state semantics, but not promoted over stronger existing lifecycle guidance.

## Notebook commits made by this run

- `92f59fc99b9de2ee81f589f14b879ec446f2c5c8` — protocol-tree node identity convention from !14203.
- `174acf6de49a8e5b7e2b5a9ddb7a593e711952a3` — retained UI/statistics data lifetime convention from !14194/!14178.
- `d8eb2ddba08a3a773a0e1e1854b08a6eee40ab94` — deferred absence-diagnostic convention from !14170.
- `07aee83bd957be2a5d1f5a072a6152f1fa6b36c2` — generated-source lexical safety convention from !14204.

## Frontier

`mr_14162.json` exists in corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054` and is merged. It was fetched only to verify the next frontier and is **not** counted as reviewed in this run. Absent newly scraped higher-numbered previously-unreviewed MRs, !14162 is the next descending candidate.