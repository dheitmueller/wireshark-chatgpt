# Wireshark Sensitive Preference Conventions

## Keep sensitive preference values runtime-only when persistence is not intended

Merged MR !5519 adds PREF_PASSWORD. The preference remains addressable through Wireshark's preference machinery and can be populated by the GUI or command line, but its value is not written to or restored from the preferences file. The review's test procedure explicitly checks that repeated use in one process works, that the on-disk preference file contains no sensitive value, and that restarting clears the remembered value.

**Implementation rule:** separate the existence of a configuration key from persistence of its sensitive value. Runtime configuration does not require durable profile storage.

**Testing rule:** verify in-process reuse, inspect the saved preference file, restart the application, and confirm the value is gone.

**Confidence:** Very high. Merged feature with explicit maintainer testing discussion.

## Do not use an in-domain empty value as an implicit reset command

Merged MR !5538 changes extcap preference handling so an empty string is preserved as a real value and adds an explicit reset-to-default action. Earlier !5527 treated empty selector storage as absent; !5538 is the later accepted behavior for settings where empty itself is meaningful.

**Implementation rule:** when empty is legal, represent default/reset separately. Do not conflate empty, missing, and default states merely because an older UI used one representation for all three.

**Confidence:** High. Merged UI/preference correction with a concrete inconsistent-runtime reproducer.
