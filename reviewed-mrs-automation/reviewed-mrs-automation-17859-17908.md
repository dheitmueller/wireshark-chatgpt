# Automated Wireshark MR review: !17859–!17908

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Review direction: descending MR number, continuing backward from the previously recorded !17909–!17958 batch.

## Exact reviewed set

!17908, !17907, !17906, !17905, !17904, !17903, !17902, !17901, !17900, !17899,
!17898, !17897, !17896, !17895, !17894, !17893, !17892, !17891, !17890, !17889,
!17888, !17887, !17886, !17885, !17884, !17883, !17882, !17881, !17880, !17879,
!17878, !17877, !17876, !17875, !17874, !17873, !17872, !17871, !17870, !17869,
!17868, !17867, !17866, !17865, !17864, !17863, !17862, !17861, !17860, !17859.

Count: **50**.

The existing tracking was consulted before selection, including `reviewed-mrs.md` and all files in `reviewed-mrs-automation/`. The historical !17571–!17620 batch remains part of the reviewed set. Selection was based on individual MR membership in the tracking, not on assuming that numeric ranges were complete.

## Review notes

Merged MRs were weighted as accepted implementation evidence; backports, abandoned work, duplicates, and superseded submissions were given correspondingly less architectural weight. No new notebook convention was added in this run because the durable findings below reinforce guidance already represented in the notebook.

### !17908 — RTCP packet length

Merged, John Thacker-authored fix. RTCP's encoded length is in 32-bit words minus one (apart from protocol-specific exceptions), not bytes. A prior conversion to a dissector table had lost that semantic. Useful corroboration that refactors which change dispatch mechanisms must preserve the units and semantic contract of values passed across the new API boundary; type compatibility alone is insufficient.

### !17907 — CBSP TCP reassembly

Merged. Updates the CBSP dissector to follow Wireshark's documented `tcp_dissect_pdus()` pattern for PDUs split across TCP segments. This reinforces the existing convention to use the standard TCP PDU reassembly infrastructure rather than hand-rolling stream-fragment handling.

### !17906 — Android Bluetooth HCI vendor support

Merged after review. Adds Android-specific HCI behavior while retaining a controller-agnostic Android dissector and preference-controlled handling rather than folding all Android semantics into one vendor's implementation. Useful corroboration for separating protocol/platform semantics from vendor-specific transport/command details when the semantics can apply across controllers.

### !17890 — androiddump truncation handling

Merged, John Thacker-authored fix. Rather than relying on a precomputed source-length test that GCC still diagnosed, the code checks the return value from `snprintf()` itself and treats truncation/failure as an explicit send error. This reinforces checking the operation's actual result at the boundary where truncation can occur, and preserving compiler-clean builds without weakening error handling.

### !17884 — display-filter semantic checking of sets

Merged, John Thacker-authored fix. Each unparsed member of an `in` set is resolved before applying the outer relation check, using non-strict resolution because literals are valid in this grammar position. The resulting diagnostics are also more semantically useful (for example, a truncated Ethernet literal is diagnosed as such instead of being mislabeled as an unknown field). This reinforces grammar-aware AST processing and the principle that validation should occur at the node whose syntactic alternatives are actually known.

### !17881 — dissector checker cleanup

Merged cleanup explicitly driven by `tools/check_dissector.py`. This is additional evidence for running the project-specific dissector checker during dissector maintenance rather than relying only on compilation.

### !17867 / !17866 — ASN.1 REAL backports

Merged release-4.4 fixes. !17867 recognizes zero-length ASN.1 REAL as the valid encoding of 0.0 instead of treating it as malformed; !17866 corrects a length assertion after accounting for the already-consumed information octet. These reinforce two existing defensive-parser rules: protocol-defined zero lengths must not be rejected merely because zero is often suspicious, and bounds assertions must be expressed in the coordinate system of the parser state after bytes have already been consumed.

### !17865 and nearby O-RAN FH CUS additions

Merged protocol-extension work. Useful primarily as implementation history; no stronger general convention was extracted beyond existing guidance on version/section-specific parsing and testing new wire encodings.

## Notebook result

No durable rule in this batch was sufficiently new to justify modifying a convention file. The strongest findings independently corroborate existing guidance on semantic API boundaries, standard TCP reassembly, AST/grammar-aware validation, compiler-result checking, project-specific static checks, and protocol-defined length semantics.

## Continuation

On the next run, rebuild the reviewed set from all tracking files again. Do not infer completeness from this filename or any other numeric range. Continue with the fifty highest-numbered corpus MRs not present in that rebuilt set.