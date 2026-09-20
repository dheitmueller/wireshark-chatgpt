# Wireshark Documentation Link Conventions

This file records durable conventions for documentation structure when generated documentation is addressed by Wireshark UI or other external consumers. Current upstream source remains authoritative.

## Treat generated help URLs and anchors as integration contracts

Documentation heading hierarchy is not purely editorial when the build system turns headings into chunked pages and anchors that application code targets. A structurally cleaner hierarchy can still be a regression if it changes the generated URL shape without updating all help-link consumers.

Merged master MR !15774, authored and merged by John Thacker, moves the UAT table sections back up one heading level in the Wireshark User's Guide. The previous logical nesting changed those sections in chunked output from individual page URLs to anchors, which broke the corresponding help buttons while issue #11266 remained unresolved. The accepted fix deliberately preserves the older generated link shape even though the deeper hierarchy was arguably cleaner.

**Implementation rule:** before changing heading levels, section IDs, chunking boundaries, or other documentation structure used by generated help, identify application/UI links that address those sections. Preserve the existing URL/anchor contract unless the consumers are changed in the same work.

**Review rule:** validate generated documentation, not just AsciiDoc source structure. Exercise relevant help buttons/deep links against the built chunked output so a documentation-only refactor cannot silently break application navigation.

**Confidence:** Very high. Merged master documentation integration fix authored and merged by John Thacker with the URL-versus-anchor failure mode stated directly in the MR.