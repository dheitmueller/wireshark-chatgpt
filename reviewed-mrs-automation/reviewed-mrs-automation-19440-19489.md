# Automated MR review: !19440–!19489

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending from newest toward older MRs. This ledger records the exact MRs counted as reviewed in this run. The previously reviewed !17571–!17620 batch remains part of the already-reviewed set.

## Exact reviewed set

!19489, !19488, !19487, !19486, !19485, !19484, !19483, !19482, !19481, !19480,
!19479, !19478, !19477, !19476, !19475, !19474, !19473, !19472, !19471, !19470,
!19469, !19468, !19467, !19466, !19465, !19464, !19463, !19462, !19461, !19460,
!19459, !19458, !19457, !19456, !19455, !19454, !19453, !19452, !19451, !19450,
!19449, !19448, !19447, !19446, !19445, !19444, !19443, !19442, !19441, !19440.

Count: **50**.

## Review notes

The batch was screened for durable coding, architecture, testing, review, and submission conventions, with merged work weighted above abandoned/superseded work and substantive maintainer review weighted above mechanical/system notes.

- **!19489 — SMB2: Added subdissector for FSCTL_DFS_GET_REFERRALS_EX.** Merged. Review caught a declaration/linkage conflict (`static` declaration following a non-static declaration) before the contributor rebased/fixed the submission. Useful corroboration that new dissector work must be validated against the current tree and project build rather than only the contributor's starting revision; no new notebook rule needed.
- **!19488 — SOME/IP-SD: Avoid format-truncation warnings.** Merged, authored/merged by John Thacker. Besides shrinking oversized numeric-string buffers to realistic bounds, it fixes wrong `snprintf` buffer-size arguments and preserves string generation when an event-group lookup fails. This reinforces existing guidance to prefer APIs/idioms that reduce size-argument bookkeeping and to audit warning fixes for latent semantic bugs rather than merely silencing the compiler.
- **!19487 — Fix lua-bit stack buffer overflow.** Merged after John Thacker approval. The final one-line guard handles the `INT32_MIN` negation corner case before negating a negative width; the change is a narrow integer-edge hardening fix and does not add a distinct convention beyond existing checked-domain/edge-case guidance.

No new standalone convention was added from this batch: the strongest reusable findings corroborate rules already present in the notebook rather than establishing a materially new Wireshark-specific convention.
