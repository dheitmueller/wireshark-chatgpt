# Profile Conventions

This file records durable architectural and behavioral conventions for Wireshark configuration profiles.

## Profile discovery must reflect the filesystem, including externally installed profiles

Do not make the visible profile list depend on a process-lifetime cache. Profiles can be created or installed outside Wireshark while it is running, including by an external installer that adds an extcap binary together with a corresponding profile. The directory structure must remain the authoritative source when profiles are listed so users do not have to restart Wireshark to discover such changes.

Merged MR !23210 is strong evidence for this contract. During its extensive Profile Dialog / ProfileModel refactor review, Stig Bjørlykke found that an early revision cached profile state such that newly created profile directories were not visible until restart. He explicitly identified external extcap installers as a supported use case and stated that reading the directory structure must be the way profiles are listed rather than caching the list. The MR went through a long cycle of profile-operation testing and fixes before ultimately merging.

**Implementation rule:** model objects may stage in-dialog edits, but refresh/discovery paths must reconcile against the profile directories at the points where current profiles are requested. Do not trade away detection of external profile creation, deletion, or renaming merely to avoid filesystem reads.

**Confidence:** Very high. Direct design rationale from the long-time profile maintainer/reviewer on an eventually merged refactor, backed by concrete regression testing.

## Treat profile-dialog edits as staged state until the user accepts them

Profile changes made inside the modal editor should be represented as model state first and committed to persistent storage on acceptance rather than performing each filesystem mutation immediately. This permits cancellation, coherent validation of multiple related operations, and correct handling of temporary states such as a profile marked for deletion or a newly imported profile.

Merged MR !23210 explicitly moved profile bookkeeping into `ProfileModel`, with changes accepted/written only when the user presses OK, deletion represented as an in-dialog state until the dialog closes, and multiple imports supported within one dialog session. The lengthy review then exercised duplicate names, delete/rename interactions, externally changed profiles, and other state-transition cases before merge.

**Implementation rule:** when a settings dialog represents a transaction over persistent objects, keep pending mutations in the model, validate the combined final state, and commit at the acceptance boundary. Avoid letting widget callbacks independently mutate persistent storage in ways that make Cancel or multi-step validation unreliable.

**Confidence:** High. Explicit design of a merged refactor, subjected to unusually extensive reviewer-driven scenario testing.