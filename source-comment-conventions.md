# Wireshark Source Comment Conventions

This file records durable conventions for source comments that arise in upstream review. Current upstream source and style guidance remain authoritative.

## Comments explain the code, not who wrote it

Do not add personal initials or author tags to source comments merely to mark who introduced or changed a block. Version-control history already provides that provenance through `git blame` / `git annotate`; embedding initials in comments creates maintenance noise and can become stale as code is refactored.

Merged MR !15892 was a substantial first-contribution enhancement to the C15 dissector. During review, Martin Mathieson asked whether `TMG` annotations were the contributor's initials and explicitly said they were unnecessary because Git history can identify which lines individual commits introduced. The contributor revised the MR along with the other style and field-name issues before merge.

**Implementation/submission rule:** reserve source comments for protocol rationale, invariants, non-obvious behavior, compatibility constraints, and other information needed to understand or safely modify the code. Leave personal authorship/provenance to version control unless a legal/licensing requirement explicitly demands attribution in the source.

**Confidence:** High. Explicit maintainer review on a merged master first-contribution MR, with the requested cleanup incorporated before merge.

## Document non-obvious protocol workarounds with the specification ambiguity they resolve

A magic adjustment is not adequately explained by a comment that merely says there is a conflict. When protocol specifications assign the same encoded value to more than one message or otherwise require contextual disambiguation, the source should preserve enough specification context for a later maintainer to understand why the workaround exists and what a better solution would have to distinguish.

Merged release-4.0 and release-3.6 MRs !9863 and !9864 were authored by Guy Harris specifically to replace terse comments around GMR-1's `0x100` disambiguation hack with a detailed explanation. The accepted comment cites the relevant ETSI specification/version and sections, identifies the duplicate message-code assignments, and explains the channel/context distinctions that appear to separate the meanings. Guy also explicitly leaves the door open for maintainers with deeper protocol knowledge to replace the hack with a better model.

**Comment rule:** for a workaround driven by ambiguous or contradictory protocol documentation, cite the exact specification/version and describe the competing interpretations or contexts. Explain the invariant the workaround is trying to preserve; do not make future maintainers reverse-engineer the rationale from a numeric offset.

**Review rule:** a high-quality XXX/TODO comment can be the correct outcome when the present implementation must be retained but its basis is uncertain. Record what is known, what is ambiguous, and what knowledge would be needed to remove the workaround.

**Confidence:** Extremely high. The explanatory changes were authored by Guy Harris and merged to two supported release branches.
