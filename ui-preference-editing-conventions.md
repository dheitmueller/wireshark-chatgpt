# Wireshark UI Preference Editing Conventions

This file records durable conventions for dialogs that edit persistent preferences while also driving an immediate action. Current upstream source remains authoritative.

## Keep tentative dialog values separate from canonical preference storage until commit

A preference editor often needs to use tentative values immediately—for example to construct capture arguments—without yet making those values the application's canonical persisted configuration. Mutating the real preference object while the dialog is still staging edits can break change detection, save/discard semantics, and later persistence code that compares the proposed value with the current value.

Merged master MR !14423, authored by John Thacker and merged by Anders Broman, fixes extcap preference persistence by removing writes to each argument's real `pref_valptr` from the dialog's `saveOptionToCaptureInfo()` path. Those premature writes made `prefs_store_ext_multiple()` believe nothing had changed, so a new preferences file was not written. The accepted code builds the capture argument set from the dialog state while leaving canonical preference storage unchanged until the explicit store path runs.

Merged follow-up !14455, again authored by John Thacker and merged by Anders Broman, makes the dialog's transaction semantics explicit: configuration mode exposes separate Save and Start actions, a forced configuration-before-capture path exposes Start, Discard closes without saving, Save commits without starting capture, and Start persists only according to the Save-on-Start policy. The accepted implementation dispatches behavior from the button role/action rather than inferring persistence from a generic dialog acceptance event.

**Architecture rule:** treat a settings dialog as a transaction. Widget state is tentative until an explicit committing action occurs. Consumers that need tentative values may copy/translate them into an operation-specific object, but should not mutate the canonical preference backing store merely to prepare that operation.

**UI rule:** when a dialog supports semantically distinct actions such as Save, Start/Apply, and Discard, encode those distinctions explicitly in the controls and handler logic. Do not make a generic Accept/Close event ambiguously stand for both committing configuration and launching an operation.

**Review rule:** test all exits: save-only, apply/start with persistence enabled, apply/start without persistence, discard/cancel, and changing a value back to its default. Verify both the immediate operation arguments and the subsequently written preference file.

**Confidence:** Very high. Two adjacent merged master fixes authored by John Thacker and accepted by Anders Broman; !14423 documents the broken change-detection mechanism directly and !14455 completes the explicit Save/Start/Discard interaction model.