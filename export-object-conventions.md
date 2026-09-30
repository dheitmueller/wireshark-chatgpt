# Wireshark Export Object Naming Conventions

This file records durable conventions for preserving protocol object names while keeping file creation safe. Current upstream source remains authoritative.

## Preserve semantic Unicode until the filesystem output boundary

Merged master MR !3993, authored by John Thacker, removes SMB's dissector-local ASCII canonicalization of Export Object hostnames and filenames. The SMB path used a restricted character allow-list and replaced other characters before the object reached the common export subsystem. The common Export Object layer already applies its own filename-safety transformation when the GUI or CLI actually writes an object.

Merged release backports !4000 and !4001 carry the same fix.

**Implementation rule:** keep object names faithful to the protocol/display value while they remain semantic metadata. Perform filename safety or normalization once, in the component that actually materializes the file, using the shared policy for that output surface. Avoid protocol-specific early canonicalization that loses Unicode or duplicates central filename policy.

**Review rule:** when a dissector transforms an object name for presumed filesystem safety, ask whether the value is still data rather than a pathname. If a later export/save layer already owns path safety, remove the earlier lossy transformation instead of maintaining two inconsistent sanitizers.

**Confidence:** Extremely high. Merged John Thacker master fix plus accepted stable backports to two maintained branches.
