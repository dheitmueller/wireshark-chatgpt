# Wireshark Source Comment Conventions

This file records durable conventions for source comments that arise in upstream review. Current upstream source and style guidance remain authoritative.

## Comments explain the code, not who wrote it

Do not add personal initials or author tags to source comments merely to mark who introduced or changed a block. Version-control history already provides that provenance through `git blame` / `git annotate`; embedding initials in comments creates maintenance noise and can become stale as code is refactored.

Merged MR !15892 was a substantial first-contribution enhancement to the C15 dissector. During review, Martin Mathieson asked whether `TMG` annotations were the contributor's initials and explicitly said they were unnecessary because Git history can identify which lines individual commits introduced. The contributor revised the MR along with the other style and field-name issues before merge.

**Implementation/submission rule:** reserve source comments for protocol rationale, invariants, non-obvious behavior, compatibility constraints, and other information needed to understand or safely modify the code. Leave personal authorship/provenance to version control unless a legal/licensing requirement explicitly demands attribution in the source.

**Confidence:** High. Explicit maintainer review on a merged master first-contribution MR, with the requested cleanup incorporated before merge.
