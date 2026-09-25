# Automated MR review ledger: 10463-10512

Reviewed using GPT-5.6 Sol.

Corpus repository: dheitmueller/wireshark-corpus-mrs

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Exactly 50 previously unreviewed merge requests were reviewed. The available reviewed-mrs.md tracking and per-run files under reviewed-mrs-automation/ were consulted before selection, including the immediately newer exact 10513-10562 ledger. Candidate membership was checked by explicit MR number rather than assuming numeric ranges were complete. The historical 17571-17620 ledger was re-opened and still contains exactly all 50 members.

Status mix: 50 merged, 0 closed/unmerged.

## Exact reviewed MR set

MR 10512, MR 10511, MR 10510, MR 10509, MR 10508, MR 10507, MR 10506, MR 10505, MR 10504, MR 10503
MR 10502, MR 10501, MR 10500, MR 10499, MR 10498, MR 10497, MR 10496, MR 10495, MR 10494, MR 10493
MR 10492, MR 10491, MR 10490, MR 10489, MR 10488, MR 10487, MR 10486, MR 10485, MR 10484, MR 10483
MR 10482, MR 10481, MR 10480, MR 10479, MR 10478, MR 10477, MR 10476, MR 10475, MR 10474, MR 10473
MR 10472, MR 10471, MR 10470, MR 10469, MR 10468, MR 10467, MR 10466, MR 10465, MR 10464, MR 10463

Count: 50 unique MRs.

## Promoted findings

- MRs 10510 and 10511: packet-derived byte spans must be validated before pointer-style TVBuff access; malformed input belongs on the packet/bounds error path while assertions encode internal invariants.
- MRs 10488 and 10505: tap completion and UI-object lifetime are distinct; destroy tap-backed model data at the owning object's lifecycle boundary.
- MR 10493: checker-driven metadata cleanup still needs semantic/protocol review; checker silence is not proof that a field change is correct.
- MR 10471: edit the authoritative ASN.1/template source and regenerate checked-in dissector output.
- MR 10474 with MRs 10489 and 10490: correctness fixes are normal stable-backport candidates; enhancements generally are not.

Full notes: review-findings-10463-10512.md.

## Next frontier

MR 10462 exists at the same corpus commit, is merged, and was inspected only as the next-frontier probe. It was not reviewed or counted in this run.
