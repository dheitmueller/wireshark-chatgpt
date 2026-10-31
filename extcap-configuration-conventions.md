# Wireshark Extcap Configuration Conventions

## Presence of configuration and need for configuration are different predicates

Merged master !9625 fixes a mandatory extcap argument that already has a valid default value. Previously the UI treated the existence of configuration as proof that interactive setup was necessary and prevented immediate capture. The accepted change separates `extcap_has_configuration()` (there are settings worth exposing in a dialog) from `extcap_requires_configuration()` (one or more required values are actually unsatisfied, taking saved preferences and defaults into account).

**Rule:** do not block capture merely because an extcap interface advertises options. Validate the *effective* values after default and preference resolution; only unresolved mandatory configuration should prevent launch or require user intervention. Keep the same semantic predicates across the capture-options dialog, interface widgets, and both Wireshark and Logray entry points. **Confidence:** high, merged master fix.

## Option-type controls must retain their established rendering

During review of !9625, Tomasz Moń identified a regression: removing `ExtArgBool::createLabel()` distorted USBPcap's boolean-flag option layout. The contributor acknowledged that the removal was incorrect. Gerald Combs separately requested `qUtf8Printable` for Qt diagnostic strings, to state the intended UTF-8 conversion explicitly.

**UI rule:** any refactor of shared option widgets must compare a real extcap's boolean, defaulted, mandatory, and text-input controls before and after, including the disabled/unchecked/flag-only states. Preserve Qt widget/label specialization where the extcap argument kind requires it, and use explicit UTF-8 conversions at logging/API boundaries. **Evidence caveat:** the source is a merged MR with substantive review feedback; verify the precise follow-up status before claiming the UI issue was fully corrected. **Confidence:** high for reviewer-identified behavior.
