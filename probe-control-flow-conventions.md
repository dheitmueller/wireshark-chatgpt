# Wireshark Recognition-Probe Control-Flow Conventions

This file records durable conventions for speculative protocol and file-format recognition extracted from accepted Wireshark review. Current upstream source remains authoritative.

## Recognition heuristics should reject incomplete candidates without throwing

A heuristic dissector is still deciding whether it owns the packet. Missing bytes are therefore ordinarily evidence that the candidate does not match, not a reason for a bounds exception to escape from the probe. Use tvbuff availability predicates for the exact bytes about to be inspected instead of hand-written unsigned length arithmetic whose subtraction can wrap around.

Merged master MR !16441, authored by John Thacker and approved/merged by Anders Broman, fixes the LBMSRS heuristic. The old code subtracted unsigned offsets and compared the result with a captured length; wraparound could defeat the check and let a later access throw. The accepted implementation uses `tvb_bytes_exist()` at each recognition boundary and returns `FALSE` when the needed bytes are absent.

**Implementation rule:** keep heuristic recognition conservative and non-throwing for ordinary truncation. Before reading a candidate discriminator or variable-length field, use the tvbuff API to ask whether that exact region exists. Avoid reimplementing bounds checks with unsigned subtraction/addition when the tvbuff API already expresses the contract directly.

**Confidence:** Very high. Merged master correctness fix authored by John Thacker and approved/merged by Anders Broman, motivated by real captures.

## Preserve explicit ordered early-exit control flow in file-recognition pipelines

Wiretap file detection is an ordered search in which each recognizer can accept, reject, or fail, and successful recognition stops the search. A generic collection/foreach abstraction is not automatically an improvement when it makes that early-exit policy awkward or requires side-channel state to suppress later callbacks.

Merged master MR !16407 refactors `wtap_open_offline()` so file handlers are attempted directly in the established order instead of constructing and passing arrays of handler pointers. During review, Anders Broman suggested GLib pointer-array helpers; the author noted that `g_ptr_array_foreach()` has no clean early-termination mechanism and then simplified the design further so the temporary pointer-array machinery was unnecessary. Anders explicitly had no objection to the revised approach, and the refactor was merged.

**Implementation rule:** for ordered recognition pipelines, prefer control flow that makes ordering and early termination obvious. Introduce a generic container/iteration abstraction only when it preserves those semantics cleanly; do not trade visible policy for callback-side bailout flags or temporary collection plumbing merely to use a generic foreach API.

**Confidence:** High. Merged master Wiretap refactor after explicit maintainer discussion about the collection/iteration alternative.

## Keep recognizers format-specific and let orchestration own ordering

When several file encodings or compression formats are recognized by independent signatures, keep each recognizer responsible for one candidate format and its own accept/reject/error result. The outer recognition path should own the order in which candidates are tried and the early-exit policy. This keeps format-specific assumptions, references, and support/error behavior local rather than interleaving them in one monolithic probe.

Merged master MR !16121, authored and merged by Guy Harris, splits Wiretap compression recognition into per-format routines such as the gzip/zlib, Zstandard, and LZ4 recognizers. It also groups the format documentation references and decompression implementation with the corresponding format-specific code. The common `check_for_compression()` path remains responsible for trying the recognizers and interpreting their tri-state result.

**Implementation rule:** structure multi-format recognition as a small orchestration layer over single-format probes. Each probe should answer only whether its own format matches, is unsupported/erroring, or does not match; it should not know the global ordering policy or duplicate other formats' recognition logic.

**Review rule:** when adding another recognized format, prefer adding one isolated recognizer and inserting it deliberately into the ordered probe sequence. Check that unsupported-format errors remain distinguishable from ordinary non-matches and that the new probe cannot consume or mutate state in a way that prevents later candidates from being tested correctly.

**Confidence:** Very high. Merged master Wiretap refactor authored and merged by Guy Harris, with the format-specific decomposition embodied directly in the accepted architecture.
