# Wireshark API Error-Reporting Conventions

This file records durable conventions for ownership of diagnostics across Wireshark API layers. Current upstream source remains authoritative.

## Report failures at the layer that owns the complete diagnostic context

When a common operation already has the information needed to describe its own failure, prefer reporting that failure from the operation layer rather than returning a collection of low-level details and making every caller reconstruct the same diagnostic. This keeps wording and behavior consistent across frontends and reduces the chance that a caller loses context or implements a subtly different error policy.

Merged master MR !16218, authored and merged by Guy Harris, first moves file-merging callers onto Wireshark's `report_message` reporting path specifically to make it possible to pull merge diagnostics into `wiretap/merge.c`. Merged master MR !16234, also authored and merged by Guy Harris, completes that direction: merge API routines report their errors internally instead of leaving error formatting to each caller, while `mergecap` uses the standard `cmdarg_err` helpers for command-line diagnostics.

This complements the broader semantic-facade rule established by merged !21861: callers should use the project's reporting facade rather than depending directly on lower-level presentation mechanics. The !16218/!16234 sequence adds an ownership rule for deciding which layer should invoke that facade.

**Implementation rule:** if a reusable operation owns the full failure context and all normal callers would report substantially the same error, report through the standard Wireshark diagnostic facade inside that operation. Return status needed for control flow, but do not force each caller to duplicate formatting and contextual interpretation merely to display the same failure.

**Boundary rule:** keep caller-specific policy at the caller. A library routine should not absorb frontend-specific wording, exit behavior, or UI decisions that genuinely differ by consumer; it should centralize only diagnostics that are intrinsic to the operation it owns.

**Review rule:** when an API returns several error out-parameters that every caller immediately converts into the same report, consider whether diagnostic ownership is at the wrong layer. Conversely, do not move reporting downward if doing so would erase meaningful caller-specific context.

**Confidence:** Extremely high. Two consecutive merged master refactors authored and merged by Guy Harris state and implement the direction explicitly.