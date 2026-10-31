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

## First-run profile bootstrap cannot assume the normal metadata index exists

Merged !9133 (John Thacker) fixes copying a global profile to a personal directory during CLI startup, before the hash of profile configuration filenames is initialized. The accepted path copies source files directly rather than filtering against an incomplete hash built only after parsing begins. **Rule:** distinguish first-run/bootstrap state from normal indexed operation, and validate profile copying before and after configuration-file discovery. **Confidence:** very high.

## Do not silently invalidate existing profiles to enforce cross-platform naming

Open draft !9117 proposed forbidding leading-dot profile names on all systems for cross-platform exports. Stig Bjørlykke questioned breaking profiles valid on Unix/Linux and suggested considering import/export warnings rather than imposing Windows restrictions on current profiles. Roland Knall acknowledged the compatibility concern and also noted .git directories being misrecognized as profiles.

**Review rule:** distinguish filesystem discovery hygiene and cross-platform archive validation from retroactively invalidating user-created profile names. Consider warnings at the actual export/import boundary and investigate how Windows imports are handled before adopting a global restriction. **Status:** substantive maintainer feedback on an *unmerged open* proposal; no new naming policy was accepted in !9117.

## Per-profile settings must actually take effect when switching profiles

Open/unmerged !9037 proposed moving SMI path/module UATs into the configuration profile. **Stig Bjørlykke** observed that existing MIB protocol registration still required restart, so changing the profile's stored values did not make the newly selected MIBs effective until restart. He requested solving unregistration/re-registration before treating these UATs as truly profile-switchable. **Review rule (provisional):** test runtime profile switching as well as command-line `-C` startup; persisted configuration alone is not enough for a live-switch feature. This MR was not merged in the corpus; do not assume the proposed behavior was accepted.
## Profile copying before configuration discovery is a special startup path

Merged **!8686**, by **John Thacker**, repairs copying a global profile into a personal profile during CLI startup when the `profile_files` hash table has not yet been initialized. The early path copies ordinary files from the global profile directory; the normal path continues selecting known profile-owned filenames. **Stig Bjørlykke** objected to making unrestricted copying the normal rule because the list exists to prevent unrelated files from becoming profile content. He accepted the scoped early/global behavior and requested reusable copy logic rather than duplicate blocks. The implementation uses a shared filesystem helper and avoids recursive directory copying.

**Rule:** distinguish bootstrap/discovery state from fully initialized profile state; make the exception explicit, prevent indiscriminate copying of arbitrary files in ordinary profile operations, and validate CLI-first startup plus standard profile export/import. Later merged **!9133** addresses related global-profile startup behavior; consult the current source and both accepted MRs before changing it. **Confidence:** high, substantive Stig/John discussion and merged fix.
