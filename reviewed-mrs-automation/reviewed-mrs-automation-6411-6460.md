# Wireshark MR review !6411-!6460

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Reviewed with GPT-5.6 Sol.

## Exact reviewed set

- !6460
- !6459
- !6458
- !6457
- !6456
- !6455
- !6454
- !6453
- !6452
- !6451
- !6450
- !6449
- !6448
- !6447
- !6446
- !6445
- !6444
- !6443
- !6442
- !6441
- !6440
- !6439
- !6438
- !6437
- !6436
- !6435
- !6434
- !6433
- !6432
- !6431
- !6430
- !6429
- !6428
- !6427
- !6426
- !6425
- !6424
- !6423
- !6422
- !6421
- !6420
- !6419
- !6418
- !6417
- !6416
- !6415
- !6414
- !6413
- !6412
- !6411

Count: 50 unique MRs. Minimum: !6411. Maximum: !6460.

Outcomes: 45 merged; 5 closed/unmerged: !6441, !6433, !6430, !6417, and !6412.

## Tracking reconciliation

- Notebook base: `5a7365223e0817c08e48285424ffca241065100f` on `automation/mr-review-6461-6510-authoritative`.
- Enumerated the review-tracking directory: 398 ordinary exact-range ledgers plus 17 irregular/gap/backfill/noncontiguous/exact-list trackers, along with the aggregate automation tracker.
- Checked all 17 irregular trackers for !6411-!6460: no candidate was present.
- Checked `reviewed-mrs-automation/reviewed-mrs-automation.md` and root `reviewed-mrs.md`: no candidate was present.
- Checked the immediately preceding `reviewed-mrs-automation-6461-6510.md`. Its only candidate reference is !6460, explicitly recorded as a metadata frontier probe and not reviewed.
- Revalidated `reviewed-mrs-automation-17571-17620.md`: it contains all 50 unique MRs from !17571 through !17620 with no omissions, so that historical batch remains preserved and counted.
- The selected set is therefore the fifty highest-numbered corpus MRs not already reviewed.

## Weighting

Merged MRs are treated as accepted evidence. The five closed MRs are retained only for review history or negative/superseded evidence and are not treated as accepted implementation precedent. Direct maintainer architecture guidance, especially Guy Harris's comments and merged changes in !6432/!6435/!6438 and his review in !6430/!6428, receives correspondingly high weight.

Next frontier (metadata probe only, not reviewed): !6410, `Move the idl directory to epan/dissectors/corba-idl.` (merged).
