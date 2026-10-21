# Review findings — backfill !15376, !701, !261

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## !15376 — Add recursion checks to ASAP, iSCSI, JXTA, MBIM, and MQTT-SN

Outcome: closed/unmerged; implementation evidence down-weighted.

The MR was created on 2024-04-23 and closed less than two minutes later. Although its title and description identify a backport of recursion-check commit c49e1f2ceacac3e7a808f95c88d4eabf81c996e5, the recorded MR contains 581 commits and 848 changed files. Its first commit is an unrelated EPLv2 change, which is direct evidence that the submitted branch/base ancestry did not represent the intended patch.

The accepted implementation already exists in merged !14643 on master and merged release backports !14645, !14646, and !14647. Each clean MR is one commit touching five files. !15376 contributes no new architectural precedent; it corroborates the rule that a wildly exploded MR should be treated as a branch/base ancestry failure and recreated from the correct target.

## !701 — Backport dumpcap message fix

Outcome: closed/unmerged; implementation evidence down-weighted.

Guy Harris created and closed this MR within seconds. It contains 574 commits and 593 changed files for what should have been a small backport. Guy's substantive discussion explicitly says not to reopen the MR. The first commit is the intended macOS dumpcap capture-permission-message update and says it is backported from commit 4fd7983b04695bfc1ccf83b49559074bfd3a80d1.

That source commit is the one-commit/two-file merged master MR !700, which is the authoritative implementation evidence. !701 is useful only as strong Guy Harris workflow evidence that an ancestry-exploded backport should be abandoned rather than reviewed or merged as-is.

## !261 — Fix indentation

Outcome: closed/unmerged; implementation evidence down-weighted.

Guy Harris created and closed this MR in under a minute. It contains 810 commits and 580 changed files even though its intended first commit is only "ncp: fix indentation." Guy added two discussion notes explicitly warning that the MR should not be reopened or merged.

The clean successor !262 targets master-3.0 and merged as one commit touching one file. It supplies the accepted implementation evidence, while !261 supplies only submission/workflow evidence.

## Durable result

All three MRs exhibit the same pathology across widely separated dates: a tiny intended backport or cleanup appears as hundreds of unrelated commits/files because the source branch or base is wrong. The durable submission convention is recorded in submission-conventions.md.

The review also uncovered a corpus-selection pitfall: a normal file-content read can yield an empty body for these large JSON files even though the Git tree records a large blob and a direct blob read returns valid content. That rule is recorded in corpus-selection-conventions.md.

No SMPTE ST 291 / VANC packet type was encountered.
