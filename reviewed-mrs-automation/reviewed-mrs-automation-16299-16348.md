# Wireshark MR review automation ledger: !16299-!16348

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Notebook starting commit: `0690f53f9e7e11543cac85bcd32463b72392d959`

Selection method: enumerated the complete recursive Git tree for the corpus commit, reconstructed the already-reviewed set from `reviewed-mrs.md` and all available per-run files in `reviewed-mrs-automation/`, preserved and counted the historical !17571-!17620 batch, subtracted that exact set from the corpus MR set, sorted the remainder numerically descending, and selected the first 50. No numeric range was assumed reviewed merely because another MR in that range appeared in a ledger.

Exactly reviewed in this run (50 MRs):

- !16348
- !16347
- !16346
- !16345
- !16344
- !16343
- !16342
- !16341
- !16340
- !16339
- !16338
- !16337
- !16336
- !16335
- !16334
- !16333
- !16332
- !16331
- !16330
- !16329
- !16328
- !16327
- !16326
- !16325
- !16324
- !16323
- !16322
- !16321
- !16320
- !16319
- !16318
- !16317
- !16316
- !16315
- !16314
- !16313
- !16312
- !16311
- !16310
- !16309
- !16308
- !16307
- !16306
- !16305
- !16304
- !16303
- !16302
- !16301
- !16300
- !16299

## Durable findings

- Merged !16344, authored and merged by John Thacker, shows that after a count/length is clamped or normalized, all downstream shift/count/offset arithmetic must use the canonical normalized value rather than reusing the original out-of-range input. This was added to `arithmetic-safety-conventions.md`.
- The merged C99 conversion series in !16339, !16338, !16336, !16335, !16333, !16331, !16325, !16324, and !16300 strongly corroborates the notebook rule to prefer standard C types for Wireshark-owned state. Closed !16302 is useful negative evidence: John Thacker explicitly rejected mechanically changing callbacks defined in terms of GLib `GHashFunc`/`GEqualFunc` contracts to a merely representation-compatible C type. Existing `c-type-conventions.md` already captures the dependency/API-boundary exception, so no duplicate rule was added.
- Merged !16337 shows a CMake failure mode where prefixing an empty optional source/header variable produces a directory path that is then handed to file-oriented tooling. This corroborates the broader build-system principle that optional inputs must be tested in their semantic form before constructing dependent paths.
- Merged !16326 distinguishes Asciidoctor external/attribute URLs (`link:`) from document cross-references (`xref:`), fixing invalid-reference warnings.
- Merged !16304 shows that callers should not duplicate diagnostics already owned by a shared option parser; the common parser can report the actual long option more accurately.
- Merged !16303 replaces a bespoke debug macro with Wireshark's logging domain/severity infrastructure, corroborating existing logging guidance.
- Merged !16345 and !16340 reinforce fixed-array capacity and ordinary zero-based indexing discipline in TCP SACK state.
- Merged release backport !16299 corrects DHCP Option 82 suboption 19 from an integer-valued field to a presence-only field, reinforcing that registered field types must match protocol semantics rather than an assumed wire representation.

Closed/unmerged work, including !16320 and !16302, was deliberately down-weighted relative to merged changes; !16302 was retained only as strong maintainer rationale for why the rejected type conversion should not be performed.
