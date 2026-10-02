# Wireshark Source Comment Conventions

This file records durable conventions for source comments that arise in upstream review. Current upstream source and style guidance remain authoritative.

## Comments explain the code, not who wrote it

Do not add personal initials or author tags to source comments merely to mark who introduced or changed a block. Version-control history already provides that provenance through `git blame` / `git annotate`; embedding initials in comments creates maintenance noise and can become stale as code is refactored.

Merged MR !15892 was a substantial first-contribution enhancement to the C15 dissector. During review, Martin Mathieson asked whether `TMG` annotations were the contributor's initials and explicitly said they were unnecessary because Git history can identify which lines individual commits introduced. The contributor revised the MR along with the other style and field-name issues before merge.

**Implementation/submission rule:** reserve source comments for protocol rationale, invariants, non-obvious behavior, compatibility constraints, and other information needed to understand or safely modify the code. Leave personal authorship/provenance to version control unless a legal/licensing requirement explicitly demands attribution in the source.

**Confidence:** High. Explicit maintainer review on a merged master first-contribution MR, with the requested cleanup incorporated before merge.

## Comments should explain protocol intent rather than point at another implementation

Merged Git dissector MR !1313 initially used a comment describing which existing field implementation to mimic. Jonathan Nieder asked for the comment to state the protocol intent instead: why the sideband byte is recognized heuristically, what ambiguity remains, and what behavior the parser is assuming. The implementation mechanics are visible in the code and can evolve independently.

**Implementation rule:** use comments for protocol rationale, ambiguity, invariants, and non-obvious assumptions. Avoid comments whose main purpose is to name another function or field as the implementation pattern; that detail is usually better expressed by the code itself and can become stale after refactoring.

The same review also removed an unrelated cosmetic edit and refined semantic naming, reinforcing that explanatory comments are most useful when the surrounding change remains focused.

**Confidence:** High. Direct review was incorporated before the master MR merged.

