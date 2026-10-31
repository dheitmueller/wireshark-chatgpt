# Wireshark Preference Edit Transaction Conventions

This file records durable conventions for preference-dialog staging, application, rejection, and change effects. Current upstream source remains authoritative.

## Treat preference dialogs as transactions: stage first, publish only on accept

Opening or editing a preference dialog must not mutate the live analysis state that the dialog is supposed to configure. A rejected dialog must leave packet/frame state unchanged, and accepting a dialog should report and trigger refresh work only when the staged value actually changes live state.

Merged master MR !12851, authored and merged by John Thacker, fixes the TCP sequence-analysis override preference. The old UI wrote the selected override directly into each `frame_data` while the dialog was still open, so pressing Reject left the mutation in place without the redissection that normally accompanies an accepted preference change. The accepted implementation instead stashes the affected frame pointers while the modal dialog is active, applies the selected value while unstashing on acceptance, marks the preference effect only for frames whose value actually changed, and frees the staged list during reset/cleanup. The MR also avoids forcing a full redissection merely because the preference dialog was opened.

**Implementation rule:** separate editable/stashed preference state from live state. UI callbacks may update the staged representation, but irreversible or externally visible state changes belong to the accept/unstash path. Rejection/reset must discard staging without publishing it.

**Effect rule:** compute preference-change/effect flags from changes actually committed to live state, not from dialog lifecycle events. An accepted no-op should not pay for unnecessary redissection, and a rejected edit must never leave partially applied state behind.

**Lifetime rule:** if staging temporarily stores pointers to live objects, justify their validity for the entire edit transaction (for example, a modal dialog that prevents the backing capture from being replaced); otherwise stage stable identifiers instead.

**Confidence:** Extremely high. Merged master correctness/performance fix authored and merged by John Thacker with the transactional failure mode and effect-handling rationale stated explicitly.

## A preference controlling the preferences dialog still follows the dialog's commit boundary

Merged !9106 (Moshe Kaplan, reviewed and merged by Martin Mathieson) exposes an adjustable debounce timer for expensive UI searches in Supported Protocols and Preferences → Advanced, with a 400 ms initial default and editing through `pref_stashed`. Martin observed that a changed timer did not take effect *inside the same open preferences dialog* until OK was pressed, and requested a clear explanation of the option's intended scope. The author explained that input debouncing is appropriate only when processing can consume meaningful CPU; short, inexpensive filtering need not be delayed.

**Rule:** a preference editing widget should not bypass the normal accept/unstash transaction merely because that preference governs the same dialog. Document whether changes apply immediately or on OK, describe the exact consumers, and keep default latency responsive while avoiding expensive per-keystroke work. Test rapid typing, accept/cancel and the next reopening. **Confidence:** very high, merged UI changes and specific Martin Mathieson review.
