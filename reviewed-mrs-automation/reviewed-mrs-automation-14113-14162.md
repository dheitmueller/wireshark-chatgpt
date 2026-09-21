# Reviewed MRs automation run: !14113–!14162

## Corpus snapshot

- MR corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `2e64d387b5315b3aeb8779dba6864e7344cfe87e`

## Selection method

The already-reviewed MR set was reconstructed from the available tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md` and the per-run files in `reviewed-mrs-automation/`. The historical !17571–!17620 batch remains part of the reviewed set. The corpus and notebook had not advanced since the preceding exact !14163–!14212 run, whose reconciliation explicitly left !14162 as the next unreviewed frontier. Exact MR-number set subtraction therefore selected the fifty highest-numbered corpus entries not already reviewed; no numeric range was assumed reviewed solely from neighboring ledger entries.

## Exact MRs reviewed

!14162, !14161, !14160, !14159, !14158, !14157, !14156, !14155, !14154, !14153, !14152, !14151, !14150, !14149, !14148, !14147, !14146, !14145, !14144, !14143, !14142, !14141, !14140, !14139, !14138, !14137, !14136, !14135, !14134, !14133, !14132, !14131, !14130, !14129, !14128, !14127, !14126, !14125, !14124, !14123, !14122, !14121, !14120, !14119, !14118, !14117, !14116, !14115, !14114, !14113.

Exactly 50 MRs were reviewed in this run.

## Status weighting

- 48 merged MRs.
- 2 closed/unmerged MRs: !14127 and !14123.
- !14127 was down-weighted because the first Logray Follow implementation was closed and superseded by merged !14149.
- !14123 was a self-closed one-line documentation typo and provides little durable review evidence.
- Merged master changes and maintainer-authored/maintainer-reviewed changes were weighted most heavily. Guy Harris's architectural comments on !14117 were given particularly high weight.

## Durable findings promoted to the notebook

### Display-filter language invalidation and lifecycle

Merged !14117 (John Thacker) restores display-filter revalidation after macro reload. Guy Harris clarified that the relevant event is not narrowly “fields changed” or “a dissector UAT changed”: it is that something happened that can change the syntax or validity of display filters. Named fields may be generated from sources such as FreeRADIUS files, and macros or other user-defined syntax can independently change the language. The durable rule is to model invalidation signals by their semantic effect on consumers, with every producer of language changes feeding the same path.

Merged !14134 (John Thacker) establishes a complementary compiled-filter lifecycle rule. Recompiling unchanged filter text during live-capture tail processing can change semantics because compilation may incorporate hostname lookup results, macro definitions, and selected-frame field-reference context. Preserve the existing compiled filter while merely processing newly arrived packets; recompile at explicit rescan/reload/retap/new-file or other semantic invalidation boundaries.

Merged !14143 (John Thacker) unifies the allowed-character predicate for both display-filter macro syntaxes and rejects invalid macro-name characters at the lexical boundary instead of producing the misleading later error that a larger invalid macro name does not exist. Merged !14124 further distinguishes a missing/null macro argument from a literal empty string, diagnosing the invalid call directly rather than substituting empty text and relying on a downstream parse error.

These findings were added to `dfilter-conventions.md` in commit `7e83117f691ce59b9af605103abbe29ff01c4f1d`.

### Textual byte parsing

Merged master !14152 (John Thacker, merged by Anders Broman) fixes `hex_str_to_bytes()` so malformed odd runs of three or more hex digits are rejected instead of being decoded by pairing digits and implicitly padding the final leftover nibble. Single-digit forms remain deliberately supported. Merged release-4.2 backport !14153 carries the same behavior. The durable rule is to reject structurally malformed byte text instead of inventing an ambiguous recovery/padding interpretation.

This finding was added in `textual-byte-parsing-conventions.md` in commit `f2065e10fcfa1b63b2445e5e3ad4647ab00829d5`.

### Cross-platform path handling

Merged master !14155 (John Thacker, merged by Anders Broman) replaces hand-written last-separator searches in ring-buffer filename handling with `g_path_get_basename()`, `g_path_get_dirname()`, and `g_build_filename()`. Windows accepts more than one valid path separator form, so searching only `G_DIR_SEPARATOR` is not a complete pathname parser. Release backports !14161 and !14162 carry the same correction.

This finding was added in `filesystem-path-conventions.md` in commit `b4714c88ca730b5886ea4ed8c6a7971ead72edb9`.

## Strong corroborating findings not duplicated in notebook rules

- !14150 fixes `gint64 temp = c * 48828125` by making the constant `INT64_C(48828125)`, ensuring the multiplication itself occurs in 64-bit arithmetic. This directly corroborates the existing arithmetic rule that a wider destination does not retroactively widen an overflowing intermediate expression.
- !14160 rejects an invalid RTMP chunk size before it can drive a non-progress/infinite-loop condition, corroborating the existing parser-progress/minimum-length guidance.
- !14158, !14141, and !14133 reinforce the established `FT_BOOLEAN`/mask/`BASE_NONE` typed-item checker conventions.
- !14157 and !14156 make tests conditional on the optional Mongo zstd capability rather than failing when the feature is legitimately absent, corroborating dependency/capability-aware testing practice.
- !14146 and !14139 reinforce selective heuristic recognition: cheap framing checks are followed by stronger validation before claiming payloads.
- !14129 reinforces preference/recent-setting migration behavior: compatibility fallback should still return a real “no such setting” result for garbage input rather than falsely reporting success.
- !14118 treats a multi-line capture error as multiple log records when stripping display-only prefixes, rather than assuming one prefix per aggregate string.
- !14114 includes direct platform testing of the new macOS setup prefix in a VM and successfully builds TShark from the alternate prefix.

## Frontier

`mr_14112.json` exists at the corpus commit above. It was fetched only to verify that the corpus continues and was **not** counted as reviewed. Unless a later corpus snapshot introduces higher-numbered unreviewed MRs, !14112 is the next descending candidate.
