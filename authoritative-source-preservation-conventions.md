# Wireshark Authoritative-Source Preservation Conventions

This file records durable conventions for imported specification material, generated protocol sources, and other files that contain both upstream source text and Wireshark-owned wrapper content. Current upstream source and the governing external specification remain authoritative.

## Distinguish verbatim imported specification text from Wireshark-owned wrapper text before cleanup

When a source file is generated from or embeds an authoritative specification, do not apply ordinary cosmetic cleanup blindly across the entire file. Text copied from the upstream specification may need to remain verbatim even when its spelling, wording, or style would normally be changed in Wireshark-owned code or comments.

Merged master MR !14353, authored and merged by Guy Harris, adjusts SocketCAN's generated Linux UAPI material while explicitly separating spelling cleanup in Wireshark-owned introductory comments from the imported kernel text. Guy notes that the material copied from Linux headers should match the authoritative source rather than being independently edited for style.

**Implementation rule:** identify which portions of a generated/imported file are authoritative-source material and which are local scaffolding before applying lint, spelling, formatting, or wording changes. Keep imported specification text synchronized with its source; make local cosmetic improvements only to content Wireshark owns unless the upstream source itself is also being corrected and regenerated.

**Review rule:** a seemingly harmless spelling or formatting change in generated protocol material should trigger the question “is this text ours, or is it an intentionally verbatim copy of the authoritative source?”

**Confidence:** Extremely high. Merged master source-preservation change authored and merged by Guy Harris.

## Treat unexpected generated-data deletions as a source or generator regression until explained

Updating to a newer external specification should not be judged only by whether regeneration succeeds. A large or surprising disappearance of previously known identifiers can indicate that the authoritative source reorganized tables or sections and the generator still assumes the old layout.

Merged master MR !14295 updates DICOM generated material from the 2021b to 2024a release. During review, Guy Harris noticed that previously present OIDs disappeared and explicitly asked whether older protocol versions would stop dissecting. Investigation showed that the standard had moved the “Well-known frames of reference” UIDs into a different source table, so the generator/source inputs had to be adjusted rather than accepting the deletion as a consequence of the upgrade. The same update also moved generated-source markers far enough to expose an arbitrary license-check scan horizon, which was corrected separately in merged !14302.

**Implementation rule:** when refreshing generated protocol tables, inspect the generated diff for removals as well as additions. Reconcile unexpected removals against the new authoritative specification and generator extraction logic; source moves, table splits, or renamed sections should be handled explicitly so historical values do not silently disappear.

**Review rule:** unexplained mass deletion from generated registries is a red flag even when the new source is authoritative and the generator exits successfully. Prefer generator/source fixes that explain the new structure over manually restoring generated output.

**Confidence:** Extremely high. Merged master standards update with the missing-data risk identified by Guy Harris and corrected before acceptance.

## Identify generated-versus-hand-maintained boundaries at the artifact level

Do not assume an entire protocol subsystem is either generated or hand-written merely because one prominent file in that subsystem is generated. Source-of-truth boundaries can differ between registration tables, value definitions, packet parsing code, and local customization layers. A correct fix depends on identifying which artifact actually owns the behavior being changed.

Merged master MR !14100 fixes X11 `ConfigureWindow` dissection. During review, Guy Harris explains that Wireshark's X11 support is mixed: `x11-register-info.h` is generated from protocol descriptions, while the corresponding dissection logic in `packet-x11.c` is hand-coded. Editing the hand-maintained dissector was therefore correct for the immediate bug. Guy also notes the preferable longer-term architecture: generate mechanical dissection from the XML description and use an explicit configuration/override layer, analogous to ASN.1/DCE-RPC `.cnf` handling, for intentional Wireshark-specific behavior.

**Implementation rule:** before changing generated-looking protocol code, determine the source-of-truth boundary for the exact behavior. Modify authoritative inputs or generators for generated regions, and modify hand-maintained code directly where that code remains the owning source. Do not patch generated output merely because it is nearby, and do not force a hand-maintained behavioral fix into a generator that does not own that behavior.

**Architecture rule:** where practical, keep mechanical protocol structure derived from the authoritative specification and express deliberate local deviations through a documented override/configuration mechanism. This makes regeneration reproducible while preserving intentional presentation or compatibility behavior without ad hoc edits to generated output.

**Review rule:** when a subsystem mixes generated and manual artifacts, reviewers should ask which file is authoritative for each changed behavior and whether regeneration would overwrite the proposed change. The answer may legitimately differ within one protocol dissector.

**Confidence:** Extremely high. The ownership/generation boundary and preferred future architecture were explained directly by Guy Harris on a merged master fix.
