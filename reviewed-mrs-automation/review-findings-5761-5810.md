# Review findings 5761-5810

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

Evidence weighting: merged master changes and accepted maintainer reasoning receive the highest weight; release backports corroborate master changes; closed merge requests 5808 and 5765 are retained only as supersession or workflow evidence.

## Display-filter identifiers are compatibility surface

Merged master MR 5809 contains direct Roland Knall review that renaming existing PROFINET fields can break saved filters and profiles. His preferred migration is to keep the old field and deprecate it when practical, with an exception where the old semantic identity is genuinely wrong. Jaap Keuter's merged MR 5772 independently avoids incidental filter-name capitalization cleanup while adding a new CFM PDU and defers any wholesale naming overhaul. Promoted to display-filter-compatibility-conventions.md.

## Presentation still runs on redissection; persistent state does not

Merged master MR 5801 fixes SSH KEXINIT handling by dissecting the visible fields on every pass while keeping saved cookie and frame state behind the first-pass visited-frame guard. Promoted to dissector-state-conventions.md.

## Bounds checks precede dereference

Merged master MR 5783 fixes a PVFS2 one-byte out-of-bounds read by checking source and destination capacity before dereferencing the source pointer in the loop condition. Release MRs 5804 and 5805 carry the correction to maintained branches. The author deliberately kept the security fix narrow instead of refactoring the old parser at the same time. This corroborates existing bounds guidance.

## Master first, then explicit stable backports

During MR 5783, Jaap Keuter explicitly separated finalizing the development-tree fix from the later cherry-pick workflow. MRs 5804 and 5805 are the resulting stable backports. MR 5773 adds complementary Guy Harris review: he asks whether the Npcap reference cleanup should be backported and identifies the removal of WinPcap references as a semantic detail worth considering. Promoted to stable-branch-submission-conventions.md.

## Packet scope controls lifetime; explicit reset controls freshness

Merged master MR 5766, authored by John Thacker, removes CMS globals because a non-fatal OID decode failure could otherwise expose state from a previous packet or file, and multiple PDU entry points made selective global clearing unreliable. The accepted code stores the OID and content TVBuff in packet-scoped protocol data and resets each scratch slot immediately before the decode expected to populate it. This is the master provenance for the convention later seen in release backports. packet-state-freshness-conventions.md was updated.

## Generated dissector fixes belong in the authoritative source

Merged MR 5764 updates the MPEG PES ASN.1 conformance file so regeneration reproduces an earlier field correction. Closed duplicate MR 5765 states the same source-of-truth requirement but is superseded. MR 5763 changes the SABP ASN.1 template rather than only generated C, and release MR 5796 mirrors the MPEG PES conformance correction. These strongly corroborate generated-code-conventions.md; no duplicate rule was added.

## Additional useful evidence

MR 5810 preserves an old documentation anchor while introducing a better name after a cross-reference build failure, illustrating that documentation IDs can become compatibility surface.

MR 5802 distinguishes real internal invariants from redundant defensive checks: required SSH bignum inputs are asserted, while a proposed check already guaranteed by the called library helper was removed after Gerald Combs review.

MR 5800 and MR 5785 keep optional or dependency-limited SSH builds coherent rather than letting feature guards remove required base registration or reference unavailable crypto APIs.

MR 5784 recognizes a legal zero-payload IuUP case instead of emitting a malformed warning.

MR 5778 is earlier corroboration of the later notebook rule that high-signal compiler coverage belongs before merge while slower artifact production can be positioned separately.
