# Wireshark CLI Option Semantics Conventions

This file records durable conventions for command-line option semantics and the work those options should cause. Current upstream frontend behavior remains authoritative.

## Make expensive processing follow the option's observable semantics

A command-line flag should cause only the work required to produce its documented observable effect. Merely seeing an option associated with a downstream feature is not, by itself, a reason to enable an expensive processing stage when the invocation has no output or filter that can consume that feature.

Merged master MR !13435, authored and merged by John Thacker, fixes TShark so `--color` alone does not force packet dissection. Dissection remains enabled when output, packet details, coloring-rule reporting, or a filter actually requires it. Guy Harris's review sharpened the user-facing side of the same contract: if `--color` is supplied in an invocation with nothing to color, the appropriate behavior is to tell the user that the option has no effect rather than silently doing unnecessary dissection.

**Execution rule:** derive expensive frontend work from the effective output/filter requirements after option parsing, not from the mere syntactic presence of a feature-related flag.

**Diagnostic rule:** when a valid option is semantically inert in the selected mode, prefer a clear warning or error where that helps the user identify a mistaken invocation. Do not manufacture work solely to make an otherwise ineffective option appear meaningful.

**Review rule:** for options that influence formatting, coloring, name resolution, decryption, reassembly, taps, or dissection depth, trace the option to the actual consumer and ask whether every invocation that carries the flag needs that processing stage.

**Testing rule:** cover the option both with and without each consumer that can make it effective. For performance-sensitive frontends, verify the no-consumer form does not unexpectedly enter packet dissection or another expensive pipeline stage.

**Confidence:** Extremely high. Merged master behavior fix authored and merged by John Thacker, with direct user-facing semantic guidance from Guy Harris.