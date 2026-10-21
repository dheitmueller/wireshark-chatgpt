# Wireshark MR findings — !1–!7

| MR | Outcome / weight | Review result |
|---|---|---|
| !7 | Merged master; accepted implementation | MPLS Echo's SR IGP IPv6 FEC field was incorrectly treated as 32 bytes. The merged fix changes it to 16 bytes and moves every following subfield offset back by the same 16-byte delta. Durable lesson: when a field-width correction changes packet geometry, audit and update every subsequent cursor/offset derived from that width in the same patch. |
| !6 | Closed/unmerged; workflow evidence only | First submission of the !7 fix. Dario Lombardo asked the contributor to remove obsolete Gerrit `Change-Id`, eliminate a merge commit by rebasing, and squash the change commits. The contributor closed it and resubmitted cleanly as !7. Strong early GitLab workflow evidence, but not implementation precedent. |
| !5 | Merged master; substantive portability/build evidence | Old libgcrypt configurations compiled out all users of Bluetooth Mesh and QUIC `fragment_items` constants while leaving the constants defined, turning `-Wunused-const` into a `-Werror` failure. The accepted change moves those declarations inside the same libgcrypt version/feature guards as their uses. During review, an unrelated missing-`clang-check` CI failure was diagnosed; Dario Lombardo recommended fixing that infrastructure separately, which became !12. |
| !4 | Merged master; scanned | Corrects a copied filename in a Qt header comment. Editorial/source-hygiene only; no new durable convention. |
| !3 | Merged master; historical workflow/documentation evidence | Begins the Developer's Guide migration from Gerrit to GitLab and documents the triangular workflow: canonical upstream → local clone, local topic branch → contributor fork, contributor fork → upstream merge request. Current workflow details have evolved, but the separation of canonical upstream, contributor fork, and local work remains useful context. |
| !2 | Merged master; strong testing/submission documentation | Updates `README.dissector` for GitLab and retains explicit requirements to test dissectors with fuzzing/randpkt. It states that a new dissector normally will not be accepted without a sample capture and notes that sample captures are used by automated fuzz testing. This is unusually direct project-documentation evidence for capture-backed protocol submissions. |
| !1 | Merged master; foundational CI/workflow evidence | Introduces GitLab merge-request CI jobs based on the previous Buildbot Petri Dish checks, switches project scripts to Python 3, removes the Gerrit-specific `commit-msg` Change-Id hook, and makes commit validation reject legacy `Bug:` / `Ping-Bug:` trailers in favor of GitLab issue references. Durable lesson: forge migrations require CI and commit-validation semantics to migrate together; stale review-system metadata should not survive merely because source code moved. |

## High-value synthesis

### Optional dependency guards define a compile-time semantic region

!5 shows that it is insufficient to guard only the code that *uses* an optional-feature object. If the declaration itself becomes unused when the feature/version is absent, strict-warning builds can still fail. Put feature-only declarations, helper tables, and constants under the same capability/version predicate as their consumers unless they have meaningful unconditional use.

### Sample captures are review and fuzzing assets

!2 is direct accepted project documentation: new dissectors are expected to be tested, representative sample captures are normally expected for acceptance, and those captures feed automated fuzzing. This strongly corroborates the notebook's later sample-capture convention.

### Submission history should be reviewable, not merely mergeable

Closed !6 is useful negative evidence. The accepted successor !7 was reached by removing Gerrit-specific metadata, rebasing away merge history, and presenting the actual change cleanly. This is early corroboration of the later topic-branch/rebase/squash guidance.

### Protocol-tree geometry must track wire geometry

!7 fixes not just the field length but every downstream offset. A corrected width that leaves following offsets untouched is still a broken parser.

## VANC

No SMPTE ST 291 / VANC packet type was encountered.
