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

## Classify failures by ownership before telling users where to report them

A low-level operation can fail because of Wireshark, a dependency, the local environment, or a remote peer. User-facing diagnostics should preserve that distinction. In particular, do not append generic “report this as a Wireshark bug” guidance to a recognized networking/remote-host failure merely because the error surfaced inside a Wireshark helper.

Merged master MR !13828, authored and merged by Guy Harris, changes `dumpcap` handling for remote-capture connection failures. Errors such as the remote host terminating the connection and libpcap text beginning “Is the server properly installed?” are treated as remote-capture/networking failures and accompanied by guidance about the remote host rather than Wireshark-bug-report instructions. Guy's accepted release-3.6, release-4.0, and release-4.2 backports !13831, !13830, and !13829 carry the same diagnostic classification.

**Implementation rule:** map recognizable failure classes to the component or environment that can plausibly own them. Preserve the original useful low-level message, then add contextual guidance for the remote system, dependency, permissions, local environment, or Wireshark itself as appropriate.

**Review rule:** whenever a diagnostic suggests filing a Wireshark bug, check the conditions that reach that text. Known connection failures, peer termination, missing remote services, permission failures, and other environmental outcomes should not be mislabeled as internal defects.

**API-design note:** repeated parsing of dependency error strings is a sign that the lower-level API may lack sufficiently structured error classification. Use the best available classification at the Wireshark layer, but prefer typed/semantic dependency errors when the lower-level API makes them available.

**Confidence:** Extremely high. The master fix and three stable backports were authored/merged or directly propagated by Guy Harris, and the MR rationale explicitly distinguishes remote/environmental errors from Wireshark bugs.

## Check dependency API return values even when failure appears impossible today

A dependency routine that returns status is defining a failure contract. Wireshark should honor that contract rather than silently discarding the result because the current call sequence makes the documented failure modes seem unlikely or unreachable. Library implementations, platform behavior, and future call ordering can change, and an ignored failure tends to turn a useful root-cause diagnostic into incorrect state or a later secondary failure.

Merged master MR !13585, authored and merged by Guy Harris, makes `dumpcap` check whether `pcap_stats()` succeeds even though failure was considered unlikely. Merged master MR !13588, also authored and merged by Guy, checks the return values of libpcap configuration routines called between `pcap_create()` and `pcap_activate()` and reports failures instead of assuming those setters succeed. Merged master MR !13589, authored by Guy and merged by John Thacker, adds another such check even though the dependency's then-current implementation could only fail for an already-activated `pcap_t`, while Wireshark was intentionally calling it before activation.

**Implementation rule:** if an external API exposes a success/error return, inspect it unless the API contract explicitly says the value is ignorable. Preserve the dependency's useful error text/context at the point where the failing call is made.

**Review rule:** treat comments such as “this cannot currently fail here” or “failure is unlikely” as reasons to verify the dependency contract, not as automatic justification for dropping the status. If the invariant is genuinely guaranteed by Wireshark, checking still documents and protects the boundary cheaply in many cases.

**Testing rule:** where practical, exercise dependency failures or mocked error returns so the error path is not merely syntactically checked. At minimum, verify that a failed configuration/statistics call cannot leave the caller behaving as though valid results were produced.

**Confidence:** Extremely high. Three adjacent merged master changes, all authored by Guy Harris, explicitly establish the policy across libpcap calls; two were also merged by Guy himself.