# Automated Wireshark MR review ledger

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection policy: rebuilt the already-reviewed set from the per-run automation ledgers and existing tracking, preserving/counting the historical !17571-!17620 batch. Selected the 50 highest-numbered corpus MRs not already present in that set. This batch is intentionally non-contiguous because !26348-!26393 had already been reviewed in earlier runs.

Reviewed MRs (exactly 50):

- !26412
- !26411
- !26410
- !26409
- !26408
- !26407
- !26406
- !26405
- !26404
- !26403
- !26402
- !26401
- !26400
- !26399
- !26398
- !26397
- !26396
- !26395
- !26394
- !26347
- !26346
- !26345
- !26344
- !26343
- !26342
- !26341
- !26340
- !26339
- !26338
- !26337
- !26336
- !26335
- !26334
- !26333
- !26332
- !26331
- !26330
- !26329
- !26328
- !26327
- !26326
- !26325
- !26324
- !26323
- !26322
- !26321
- !26320
- !26319
- !26318
- !26317

Review notes:

- Merged work was weighted more heavily than closed/unmerged or superseded work.
- Maintainer-authored and maintainer-reviewed changes were weighted according to reviewer authority.
- !26412 is a useful continuation of the eCPRI/O-RAN handoff work: when a nested dissector claims a frame, the parent still has an API contract to return the correct consumed offset and should not hide parent fields merely because a child dissector was invoked.
- The strongest reusable lessons in this batch were already represented by existing notebook conventions (dissector handoff/return-value contracts, parser bounds and forward progress, semantic state identity, and regression-driven validation). No duplicate convention text was added solely to manufacture a notebook change.
