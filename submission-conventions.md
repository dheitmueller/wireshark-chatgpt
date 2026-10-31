# Wireshark Submission Conventions

This file records durable merge-request and commit-presentation conventions extracted from upstream maintainer review. Current upstream contribution guidance and CI checks remain authoritative.

## Put the dissector/component name before the colon in commit and MR subjects

Wireshark subjects use the component being changed as the prefix. For dissector work, the protocol/dissector name belongs before the colon; a generic action prefix such as `fix:` is ambiguous and does not identify the affected component.

Merged MR !26145 began with a generic `fix:` subject. John Thacker explicitly asked the contributor to change both the MR title and the commit subject to the form `QUIC: Fix length of payload buffer passed to DATAGRAM dissector`, explaining that Wireshark requires the dissector name before the colon and that `FIX` itself is a protocol/dissector name (Financial Information Exchange). The contributor updated the title/commit and the MR was merged by John.

**Submission rule:** use `Component-or-Dissector: brief summary` for the subject, with the actual Wireshark component/protocol before the colon. Do not use generic conventional-commit prefixes such as `fix:` in place of the component name.

This sharpens the existing notebook rule that commit messages use a short `component: brief summary` subject followed by a blank line before any longer body.

**Confidence:** Very high. Direct maintainer instruction on a merged first-contribution MR, with the requested form adopted before merge.

## A green pipeline only validates the code actually present in the MR diff

Passing CI is not evidence for a feature if rebasing, force-pushing, or commit reconstruction accidentally drops the feature from the submitted diff. Before declaring an MR ready, inspect the current GitLab diff itself and verify that it contains the intended implementation, its tests, and no unrelated or stale changes.

In still-open MR !26280, the contributor reported a clean rebase and green CI for address-masking work. Pascal Quantin inspected the actual MR diff and pointed out that it contained only an unrelated `packet-trdp.c` include change, so CI was not testing the claimed feature at all. Later review by Pascal and Jaap Keuter also identified unrelated changes and accidental removal of existing CSV formula-escaping behavior.

**Submission rule:** after rebases, force-pushes, or history cleanup, review the MR's current diff as a reviewer would. Confirm that all intended files and tests remain, unrelated changes are absent, and existing behavior outside the feature scope has not been silently reverted. Treat pipeline status as validation of that verified diff, not as a substitute for verifying the diff.

**Confidence:** High for the submission/review rule. The implementation MR remained open and unresolved in the reviewed corpus snapshot, so its proposed masking design is not accepted evidence; the diff-integrity correction itself came from Pascal Quantin and is independent of whether that feature ultimately merges.

## Keep capture artifacts out of the repository root

Packet captures used to demonstrate a bug should normally be attached to the issue, and captures that become regression fixtures should live in the repository's established test-data structure. The repository root is not an acceptable dumping ground even when the file is relevant to testing.

Merged MR !26325, authored by John Thacker and merged by Gerald Combs, removes a capture accidentally committed at the repository root. The commit explicitly states that the file should have been attached to the issue and that even a test capture should not have been placed at the root.

**Submission rule:** attach reproducer captures to the issue/MR unless they are intentionally being added as maintained test data; when adding a test fixture, put it in the project's designated test/capture location and wire it into the test suite rather than committing it at top level.

**Confidence:** Very high. Merged repository-hygiene correction accepted by project leader Gerald Combs.

## Keep contribution-policy wording and CI enforcement synchronized

When a contribution trailer or policy keyword changes, update automated checks and their user-facing remediation text in the same change. A stale CI check can otherwise enforce a superseded convention even though the documentation says something else.

Merged MR !26304 updates the GitLab CI check from the former `AI-Assisted` trailer to `Assisted-by` after the contribution policy was renamed, and rewords the reminder to match CONTRIBUTING: disclosure is required when AI assistance was used, not unconditionally. Gerald Combs merged the correction.

**Submission rule:** treat policy docs, commit-trailer spelling, CI detection, and CI error/help text as one interface. When one changes, search for and update all enforcement and guidance consumers.

**Confidence:** Very high. Merged CI/policy synchronization change accepted by Gerald Combs.

## Use closing-keyword issue references when the MR is intended to resolve the issue

A plain textual mention establishes context but does not express the intended repository workflow. When the change is expected to close a tracked issue, use GitLab's supported closing syntax in the commit/MR message.

In open MR !26313, Jaap Keuter explicitly asked the contributor to add `Fixes #21544`, noting that this automatically links and closes the issue and pointing to Wireshark's SubmittingPatches guidance. The implementation itself remained unresolved and is not treated as accepted architectural evidence, but the submission instruction is independent of that outcome.

**Submission rule:** for a change intended to resolve an issue, use `Fixes #NNNN` (or the currently documented equivalent) rather than relying on an informal mention.

**Confidence:** High. Direct maintainer guidance tied to Wireshark's documented submission process; implementation MR remained open in the corpus snapshot.

## Prefer reverting an incomplete feature over a narrow band-aid at a release boundary

A patch that prevents the most obvious crash does not necessarily make an incomplete feature release-ready. Near a test release or final release, evaluate the feature as a whole—including every supported application that exposes it—rather than treating a localized crash fix as sufficient evidence that it should remain enabled.

MR !25157 proposed a narrow Stratoshark initialization fix for the new Find in Packet feature while explicitly acknowledging that the feature remained incomplete there. John Thacker recommended reverting the feature until after the 4.7.0 test release. The author agreed, !25157 was closed, and merged MR !25158 reverted the feature because of the known Stratoshark problems and proximity to the release.

**Review/submission rule:** when a newly added feature has known incomplete or broken behavior across supported applications close to a release boundary, prefer restoring the last known-good feature set over shipping a minimal crash-only repair that leaves the incomplete feature exposed. Reintroduce the feature after its cross-application behavior is complete and reviewable.

**Confidence:** Very high. The proposed partial repair was explicitly superseded after John Thacker's review, and the full revert was the merged project outcome.

## Record real MR prerequisites with GitLab dependency metadata

When one merge request cannot be meaningfully reviewed or merged until another MR lands, a prose note is useful context but is not the authoritative representation of that relationship. Use GitLab's explicit dependency/blocking relationship so the prerequisite is visible to reviewers and enforced by the workflow.

Merged MR !25085 depended on !25084. The author initially called that out in a discussion note; Michael Mann pointed out GitLab's explicit MR dependency field and added !25084 there, causing the prerequisite MR to be recorded as blocking !25085. Both changes later merged in dependency order.

**Submission rule:** use GitLab's dependency/blocking metadata for true MR prerequisites, in addition to any explanatory prose. This makes ordering machine-visible and prevents the relationship from being lost in discussion history.

**Confidence:** Very high. Direct maintainer review guidance on two merged MRs, followed by use of the repository's dependency mechanism before merge.

## Commit-message validators must honor Git's scissors boundary

A commit-message checker that reads Git's editable message buffer must validate the message Git will actually commit, not temporary editor content that Git itself strips. In particular, `git commit -v` appends a diff below Git's scissors marker rather than representing that content as ordinary comment lines.

Merged master MR !23374 updates Wireshark's commit-message validation tooling to recognize the `# -- >8 --` scissors boundary. The MR notes that everything below that line is removed from the eventual commit message, so validation should not treat the verbose diff as message text. Martin Nyhus authored the change and Jaap Keuter approved and merged it.

**Submission/tooling rule:** validators operating on an editable commit-message file should mirror Git's semantic message extraction: honor the scissors boundary and ignore content Git will discard before creating the commit. Do not report style violations from a `commit -v` diff that cannot appear in the final commit message.

**Confidence:** Very high. Merged master tooling correction accepted by Jaap Keuter, with the Git behavior and failure mode stated directly in the MR.

## Amend an existing MR when responding to review instead of replacing its review history

Review discussion is part of the engineering record. Opening serial replacement MRs for revisions to the same proposed change can strand unresolved questions and make it difficult for reviewers to tell which concerns were addressed.

The JSON dictionary-dissection work went through several early submissions, including closed !23008 and !23015, before merged !23053. In !23053, Michael Mann explicitly told the first-time contributor to keep the same MR when making changes so the review remains coherent and outstanding questions are not lost, and pointed to Wireshark's documented "amending a change" workflow. The implementation was eventually reworked into the existing JSON dissector and merged, but the review-process lesson is independent of that architectural outcome.

**Submission rule:** when reviewer feedback asks for revisions to the same logical change, amend/force-push the topic branch backing the existing MR rather than closing it and opening another MR. If a genuinely distinct successor is necessary, explicitly carry forward unresolved review context and link the superseded MR so reviewers do not have to reconstruct the discussion.

**Confidence:** Very high. Direct Michael Mann maintainer guidance during a first-contribution series whose final revision merged.

## Use a focused topic branch and choose MR scope by coherence, not by one-commit-per-change dogma

A merge request should be backed by a branch dedicated to the proposed change, rather than a contributor's long-lived `master`/`main` branch. Its history should be easy to review and merge. At the same time, several small changes that form one coherent component update do not need to be artificially split into separate MRs merely because they could be expressed as separate commits.

Closed MR !22090 provides explicit negative guidance from Jaap Keuter: he asked the contributor to replace an MR sourced from the contributor's `master`, and noted that unless multiple commits are strictly necessary the MR should normally contain a single focused commit, pointing to Wireshark's SubmittingPatches guidance. Because the implementation did not merge, this is weighted as maintainer workflow guidance rather than accepted code design. Merged MR !22072 supplies the complementary scope signal: Michael Mann questioned why three small related NetPerfMeter changes had been submitted as separate MRs instead of one coherent MR.

Merged !22054 also updated the Wireshark Developer's Guide around the triangular Git workflow, commit guidance, and target-branch selection for cherry-pick MRs, reinforcing that branch and history presentation are part of the supported contribution workflow.

**Submission rule:** develop an MR on a dedicated topic branch and keep its commits focused and reviewable. Prefer one coherent MR for a small related series; split work when changes are independently reviewable or have genuinely distinct purposes, not simply because every edit can be isolated mechanically. For stable-branch cherry-picks, explicitly target the intended release branch.

**Confidence:** High. Direct Jaap Keuter and Michael Mann review guidance, supported by a merged Developer's Guide workflow update; the branch-specific example in !22090 itself was closed rather than merged.

## New protocol skeletons can be reviewable with explicit limitations and reproducible examples

The initial Matter dissector in merged master MR !9865 was incomplete by design (encrypted content, application TLVs, and TCP transport were not yet supported). The author stated those limits and supplied `matter.pcapng`, allowing maintainers to review a bounded implementation. Alexis La Goutte explicitly requested an entry in `docbook/release-notes.adoc` and a squash before merge. Roland Knall, Stig Bjørlykke, and Jaap Keuter reviewed field naming, generated-data notation, and representation of remaining opaque bytes.

**Submission rule:** for a new dissector, explain what its initial version does *and does not* decode, provide a usable capture, update build registration and release notes, incorporate naming/readability review, and squash exploratory history to a coherent accepted commit when requested.

**Confidence:** Very high. Direct multi-maintainer discussion with revisions in a merged master MR.

## Automated update scope must still respect stable-branch policy

Closed/unmerged automation MR !9907 tried to update several registries and a generated ASTERIX dissector on a stable branch. Jaap Keuter objected that extending ASTERIX there violates the project's release policy. Gerald Combs acknowledged the issue, changed the update generator to update ASTERIX **only on master**, and closed the MR in favor of corrected !9915.

**Submission rule:** automated source/data maintenance must apply the same branch-scope restrictions as hand-written changes. When an updater emits generated protocol additions, restrict master-only feature growth before producing stable-branch cherry-picks. Treat !9907 as an authoritative review-policy rejection, **not** an accepted implementation.

**Confidence:** High for the branch-policy instruction: direct Jaap Keuter review and explicit Gerald Combs remediation. No implementation authority is assigned to the closed proposal.

## Separate urgent regression repair from unfinished feature work

In CoAP MR !9731, Stig Bjørlykke requested splitting a small header-field regression fix from a larger Q-Block enhancement needing a longer review. The author retained the fix in merged !9731 and moved the Q-Block extension to !9742, still a draft/open MR in the corpus.

**Rule:** Submit a narrow, independently testable correction without attaching speculative protocol behavior that might delay it. Distinguish accepted corrections from open successors. **Confidence:** Very high: direct Stig review, implemented split, merged fix.

## Review feedback can remain binding after an early merge

Merged master !9692 adds CoAP options, but **after merge** Stig Bjørlykke found missing entries in `COAP_COMMON_HF_LIST` (potential registration errors/crashes) and questions about private/vendor option interpretation. After checking the IANA registry, Stig revised his interpretation and still asked for a follow-up correction; the author submitted merged !9731, with larger draft Q-Block work separated into !9742.

**Review rule:** merge status is evidence of acceptance, not proof that no defects remained. Inspect substantive post-merge discussion and corrective successors, verify any spec/registry assertion against the actual registry, and ensure every newly declared field is included in all shared registration/initialization lists. Prefer fixing an accepted narrow regression before extending the feature.

**Sample-capture rule:** Stig requested a representative capture; Alexis La Goutte clarified that a capture uploaded directly to the MR is sufficient for review and need not be committed to `test/captures` unless it serves an actual regression test. **Confidence:** very high; direct reviewer discussion and merged corrective successor.

## A fork's default branch is not a suitable merge-request topic branch

Closed !9658 accumulated substantial Windows Delivery Optimization dissector review, including expert diagnostics, naming, release-note updates, and representative captures. Alexis La Goutte eventually required the contributor to stop submitting from fork `master` and create a dedicated source branch; the author opened successor !10124, and the original was closed. Its detailed code review remains useful **reviewer feedback**, but the original MR is not an accepted implementation.

**Submission rule:** start new work on a named topic branch in the fork, rebase against upstream when needed, and keep an MR's diff confined to intended commits. Do not spend repeated review cycles on a default-branch source whose unrelated history interferes with rebasing/squashing. **Confidence:** very high for Alexis's explicit branch instruction; the closed MR is negative workflow evidence.

## Captures for encrypted or uncommon protocols need reproduction instructions

For merged ICMPv6 EARO !9615, Alexis La Goutte requested a capture; the contributor supplied an 802.15.4 encrypted pcap **and the relevant dissector decryption-preference settings**, making the new fields reproducible. For merged Couchbase flag update !9614, Alexis asked for a pcap exercising the new bits and explicitly allowed uploading it to the MR. A small targeted sample is preferable to an unexplained bulk capture.

**Testing/submission rule:** attach representative traffic for newly decoded fields, tell reviewers exactly how to enable relevant protocol preferences, Decode As, keys, and expected field output, and separate test-fixture inclusion from merely uploading reviewer evidence. Do not include secrets from production captures; use disposable or sanitized keys and traffic. **Confidence:** high, repeated Alexis review on merged changes.

## Verify UI and platform regressions after a merge

Merged extcap fix !9625 received Tomasz Moń's specific report that its deleted Boolean-label override changed USBPcap options-dialog appearance; the author acknowledged the mistake. Merged macOS-CI addition !9662 was followed by a missing `speexdsp` build dependency report and !9679 repair. **Review rule:** retain actionable post-merge feedback and follow-up MRs, distinguishing acceptance of the main feature from claims that every UI/platform behavior was already validated. **Confidence:** high.

## Submit translation updates through the project's localization service

In closed/unmerged !9600, Alexis La Goutte directed a contributor fixing a German Qt .ts typo to Wireshark's Transifex workflow. The contributor registered and submitted the change there; the direct source-tree MR was closed. This corroborates the earlier localization process feedback around !26392 and is **not** an endorsement of merging one-off generated .ts edits.

**Submission rule:** check the current upstream localization workflow before editing generated or synchronized translations; use the project's translation service as the authoritative source unless maintainers request a different route. **Confidence:** high for explicit Alexis guidance, lower for implementation because the MR was closed.

## Change author attribution through `.mailmap`, not the generated `AUTHORS` listing

Pascal Quantin's review of merged !9418 corrected a contributor who originally tried to fix a name/email in generated `AUTHORS`. Pascal directed the source change to `.mailmap`, then asked that the corrective/revert history be squashed into **one** commit; the final diff contains the mailmap mapping only.

**Rule:** consult the generator/source-of-truth for attribution and other derived files instead of editing generated output. When review necessitates replacing a wrong file with the right source, present the final logical change as a coherent amended/squashed commit rather than leaving a revert-plus-fix sequence unless history itself is important. **Confidence:** very high, direct Pascal Quantin instructions and merged outcome.

## Split mechanically large protocol refactors and keep correctness fixes independent

Closed/unmerged PFCP !9334 carries important *review guidance*, not an accepted code precedent. Alexis La Goutte identified trailing whitespace, Clang Analyzer undefined-value/null-argument findings and Windows build issues, and suggested an IEEE 802.11-style registered dissector table for extensible IE dispatch. Anders Broman said the combined refactor was too large to review in the web interface and proposed splitting it into smaller MRs, with bug fixes in separate changes. The author agreed and submitted the first subset in merged !9363.

**Rule:** stage broad signature churn and protocol architectural changes as reviewable, separately testable increments; isolate already-needed bug fixes from optional structural work. Register extension handlers in a dissector table when independent protocol-specific dispatch benefits, rather than forcing all handlers into a monolithic switch. **Confidence:** very high for Anders/Alexis guidance and successful split, lower for the closed draft's implementation.

## Keep unrelated changes logically separate even when one MR is necessary

In closed/unmerged HTTP/3 !9330, Alexis La Goutte requested that an unrelated macOS self-signing package change be made separately and asked for a small sample capture (under 100 kB). The contributor explained that employer legal review made separate submissions expensive. Gerald Combs accepted the practical constraint *if* the HTTP/3 and packaging work were split into distinct, self-contained commits within the MR so reviewers could independently evaluate them. This is an accommodation, not endorsement of the large unmerged HTTP/3 code.

**Rule:** prefer a dedicated MR per independent feature, but when contribution approval rules force one MR, keep unrelated changes in distinct logical commits and supply a small reproducible capture for dissector features. **Confidence:** high for explicit Gerald/Alexis submission feedback; no accepted implementation evidence from the closed draft.

## Supply a small illustrative capture and distinguish protocol features in reviewable increments

For merged Metamako trailer extension !9300, Alexis La Goutte requested a sample capture (supplied) and identified a Windows build failure from an unavailable `abs` declaration; the author revised the code. For merged HTTP/3 additions !9276, Alexis suggested splitting the Datagram and WebTransport feature work into separate patches and requested a capture, even though the final snapshot still merged together. **Rule:** exercise new dissector behavior against a concise reproducer on all relevant platforms and keep separable protocol additions individually reviewable; a suggestion to split a merged MR remains a reviewer preference, not proof that the split happened. **Confidence:** High for capture/build requirements and explicit maintainer feedback.

## Check out another contributor's MR directly for reproduction

In merged !9149's discussion, Chuck Craft asked whether testing another contributor's MR required manually downloading and applying a diff. João Valverde provided the concise supported workflow: glab mr checkout 9149. That check-out was used to reproduce a previously reported dependent-frame crash across environments, though neither tester could reproduce the earlier failure in that snapshot.

**Contributor rule:** prefer a clean MR checkout using glab (or an equivalent isolated branch) for local validation instead of manually layering patches on unrelated work. Record the exact revision, platform and reproduction steps; inability to reproduce an old crash is not proof that dependency recursion and performance hazards have been resolved. **Confidence:** high for direct maintainer workflow guidance; moderate for the historical regression outcome.

## Squash merges must preserve useful diagnostic attribution

In merged !9089 (ciscodump Coverity initialization fix), author Dario Lombardo objected that squashing a single commit had substituted a vague MR title for the original commit message carrying the Coverity CID. The merger confirmed that the descriptive reference was lost. This is evidence about a **real metadata-loss failure**, not a project-wide prohibition on squash merges: it was a discussion between contributors, not an adopted policy banning squashing.

**Review rule:** when squashing, inspect the final commit message and retain bug numbers, issue links, Coverity IDs and technical rationale even when GitLab proposes a shorter MR-title-derived message. A squash operation should not sever traceability to the diagnostic it repairs. **Confidence:** moderate for generalized policy; high for the specific documented consequence.

## Use representative capture files for new capture-format metadata

Merged !9062 adds pcapng Darwin Flow ID and Wake Packet flag decoding. Martin Mathieson requested an example pcapng specifically because it would help test prospective registration callbacks for vendor/private blocks, and Alexis La Goutte asked whether the option description was backed by the pcapng draft/specification. No attached sample was established from the reviewed corpus record. **Submission rule:** protocol/file-format extensions should provide authoritative layout/spec references and small captures containing representative options (including unknown/absent options where relevant), so reviewers can verify byte widths, declared encodings and parser/extensibility behavior. **Confidence:** high for direct maintainer requests; do not claim a test file was delivered.

## Avoid unnecessary rebase churn and resolve independent reviewer concerns distinctly

Closed/unmerged !9048 received **Alexis La Goutte's** requests for an Arista TGen capture, a separate MetaMako change, and specific static-check/filter-abbreviation corrections. **Jaap Keuter** additionally asked the contributor to limit manual rebasing to roughly once daily because repeated rebases strained CI/infrastructure; the author had mistaken manual rebase for a mandatory step in the merge workflow. **Review rule:** do not mechanically rebase each time an MR changes state; respond to reviewer requests, keep protocol features and unrelated fixes separable, and supply focused captures. The closed patch is process evidence only, **not** an accepted design. **Confidence:** high for direct maintainer instructions.
