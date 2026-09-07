# Personal Upstream Review Feedback

This file tracks substantive feedback received on Devin Heitmueller's Wireshark contributions. Its purpose is stronger than general MR archaeology: **a reviewer should not have to give Devin the same feedback twice.**

## Contribution-history note

MR !26390 is Devin's **only Wireshark GitLab merge request to date**. Earlier Wireshark contributions were submitted through the project's mailing-list / patch-review workflow rather than GitLab MRs. Therefore, do not waste corpus-mining effort searching for additional Devin-authored GitLab MRs unless new ones have been submitted since this note was written.

If historical personal-review mining is undertaken, the next high-value source is Devin's older Wireshark mailing-list patch discussions. Reviewer feedback found there should be normalized into the same checklist below, with provenance preserved, just like feedback from GitLab MRs.

## Operating rule

When preparing, reviewing, or generating code that Devin is likely to submit upstream to Wireshark, consult this file and explicitly check the proposed diff against all applicable lessons before considering it ready for submission.

Feedback on Devin's own contributions has elevated practical significance even when it has not yet been proven to be a universal Wireshark convention. Preserve provenance and scope so reviewer-specific or context-specific advice is not incorrectly generalized.

For each useful review comment on Devin's contributions, record:

- MR, mailing-list thread, patch, or other provenance and reviewer.
- What the reviewer requested or objected to.
- The resulting change, when known.
- Scope/context.
- Classification: `personal-check`, `likely-project-convention`, or `confirmed-project-convention`.
- A concrete pre-submission check phrased so it can be applied to future diffs.

Authoritative maintainer feedback, especially from Guy Harris, should receive correspondingly high weight. Merged/accepted outcomes are stronger evidence of accepted implementation than abandoned or superseded implementations, while reviewer corrections from unmerged/unaccepted patches can still be valuable negative guidance.

## Current checklist

### Protocol-qualify internal field and subtree identifiers

**Provenance:** MR !26390, Anders Broman.

**Feedback:** Header-field and subtree symbols should use the protocol prefix throughout; examples included `hf_op47_sdp_identifier` rather than `hf_sdp_identifier` and `ett_op47_wst` rather than `ett_wst`.

**Classification:** `likely-project-convention` (also recorded in general dissector conventions).

**Future check:** Before submission, scan new/modified `hf_` and `ett_` declarations and references. Are they consistently protocol-qualified rather than generic?

### Search Wireshark APIs before adding local parsing helpers

**Provenance:** MR !26390, Anders Broman.

**Feedback:** The custom `get_bits_buf()` helper was unnecessary; existing `tvb_get_bits()` / `proto_add_bits_item` / `proto_add_bits_item_ret_uint` APIs should be used.

**Classification:** `likely-project-convention` (also recorded in general dissector conventions).

**Future check:** For every newly introduced parsing/extraction helper, have we searched current Wireshark APIs and analogous dissectors for an existing API that already performs the operation?

### Include representative sample captures for new dissector functionality

**Provenance:** MR !26390, Anders Broman. Independently corroborated by other Wireshark reviews.

**Feedback:** Example pcaps were requested for the new ST 2110-40/VANC dissectors.

**Classification:** `confirmed-project-convention`.

**Future check:** Does the submission add or identify representative capture file(s) that exercise the new protocol/dissector functionality and important cases?

### Consider consolidating tiny related dissectors

**Provenance:** MR !26390, Anders Broman.

**Feedback:** Several small related ST dissectors did not necessarily need separate C files; multiple protocols may be registered from one source file.

**Classification:** `personal-check` / contextual project guidance.

**Future check:** If the change introduces multiple small related dissector files, would grouping them into one cohesive source file be clearer and more consistent with reviewer expectations?

### Present focused changes with clean commit history

**Provenance:** MR !26390, Anders Broman.

**Feedback:** The three development commits were requested to be squashed into one.

**Classification:** `personal-check` (commit structure remains context-dependent).

**Future check:** Before requesting/finalizing review, are fixup/development-history commits squashed while genuinely independent logical changes remain separate?

## Maintenance

This checklist is cumulative. Do not remove an applicable check merely because it has become habitual. If later maintainer feedback supersedes or narrows a rule, update the entry with the new evidence and rationale.
