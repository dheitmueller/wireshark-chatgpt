# Durable conventions from Wireshark MRs 5361-5410

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master work is primary evidence; stable backports corroborate it; closed !5380, !5366, and !5361 are history only.

- **CLI compatibility (!5393):** translate a legacy option at the replacement subsystem's parser, emit deprecation guidance, and avoid weakening unrelated obsolete-option policy.
- **Version gates (!5372):** express the actual minimum supported version (for example, >= 5.11), not “greater than the previous minor”, which can accidentally admit 5.10.x.
- **Parallel frontends (!5367, !5369):** audit every success and error result across duplicated CLI option state machines. A mechanical edit to tfshark accidentally dropped its syntax-error path and needed an immediate follow-up.
- **Tokenizer bounds (!5383):** APIs receiving [start,end) must get the real enclosing range. A guessed five-byte RTSP bound silently prevented the first token from being parsed.
- **Optional builds (!5379):** feature-off compilation is first-class dependency-boundary testing; optional-subsystem references must be compile-time safe when that dependency is absent.
- **Parser refactors (!5363):** carry ancillary validation/state context through new helper boundaries, not only tvbuff/tree/offset. Use NULL only where that context truly does not exist.
- **Release notes (!5394):** “previously shipped” means the previous released product users received, not an intermediate development-tree value.
- **Skipped tests (!5362):** permanently skipped tests for behavior that does not exist are not useful coverage.
- **API retirement (!5381, !5382, !5386, !5387, !5395, !5397):** when an experimental parallel API is abandoned, remove remaining users, exported symbols, conversion helpers, generators/checkers, and migration tooling rather than maintaining two equivalent models.
