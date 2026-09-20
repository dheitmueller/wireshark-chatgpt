# Wireshark Dissector Naming Conventions

This file records durable conventions for dissector identifiers and user-facing descriptions extracted from accepted Wireshark review. Current upstream source remains authoritative.

## Keep machine identifiers separate from user-facing descriptions

A dissector's machine-facing name and its human-facing description serve different contracts. Machine names may be used by registration, lookup, preferences, Decode As, or other programmatic consumers and therefore need identifier-like syntax and stable semantics. Descriptions are presentation strings and may contain spaces, capitalization, and wording chosen for users. Do not overload a description into the name slot merely because an unregistered handle needs something readable in the UI.

Merged master MR !16140, authored by John Thacker and merged by Anders Broman, introduces and uses `create_dissector_handle_with_name_and_description()` so unregistered/custom-table handles can carry these roles independently. Existing call sites that had supplied phrases such as `Block Integrity Block`, `Payload`, or RPC procedure names as if they were dissector names are converted to descriptions, while consumers that display those phrases use `dissector_handle_get_description()`. Consumers that need the machine-facing identifier continue to use the dissector name.

**Implementation rule:** decide whether a string is an identifier or presentation text before choosing the dissector-handle API. Use stable identifier syntax for names; use descriptions for localized/human-readable labels and phrases. A UI need for readable text is not a reason to weaken the machine-name contract.

**Confidence:** Very high. Merged master API cleanup authored by John Thacker and approved/merged by Anders Broman, with multiple accepted call-site conversions demonstrating the intended distinction.

## Validate registered names at registration boundaries and share the validator

Names that become part of command-line, preference, display-filter, or lookup interfaces should be rejected at registration if they cannot satisfy that interface's syntax. If several registration domains intentionally have the same naming grammar, use the same validation function rather than maintaining near-duplicate character checks that can drift.

Merged master MR !16135, authored by John Thacker and approved/merged by Anders Broman, makes a NULL or empty registered dissector name an immediate development error and points anonymous callers to `create_dissector_handle()` instead. The same validation path already rejects invalid name characters. Merged master MR !16123, also authored by John Thacker and approved/merged by Anders Broman, replaces a separate preference-module-alias character loop with the common module/protocol name validator because the requirements are the same.

**Implementation rule:** enforce identifier invariants where the identifier enters the registry, with an error that identifies the correct anonymous/non-registered API when applicable. When two namespaces deliberately share a grammar, centralize that grammar in one validator so fixes and compatibility exceptions remain consistent.

**Confidence:** Very high. Two merged master changes authored by John Thacker and approved/merged by Anders Broman that directly establish both registration-boundary validation and shared-validator reuse.

## Do not register capitalization aliases for the same dissector without a semantic need

Registering the same dissector repeatedly under case variants or cosmetic aliases expands the externally visible naming surface without adding protocol semantics and can leave callers unsure which name is canonical.

Merged master MR !16122, authored and merged by John Thacker, removes duplicate STCSIG registration under both an uppercase short name and the lowercase `stcsig` identifier. The accepted implementation keeps one lowercase registered name while using the protocol's separate short/long presentation names for UI labeling.

**Implementation rule:** choose one canonical registered dissector name, normally following the prevailing lowercase identifier convention. Use protocol descriptions/short names for presentation rather than creating duplicate registered aliases solely to preserve capitalization.

**Confidence:** Very high. Merged master cleanup authored and merged by John Thacker with the canonicalization rationale stated in the MR.
