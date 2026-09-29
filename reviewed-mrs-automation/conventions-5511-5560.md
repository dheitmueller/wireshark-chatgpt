# Wireshark conventions from MRs 5511-5560

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

This run reviewed exactly !5560 through !5511 after reconciling all review tracking.

## Platform I/O types
Guy Harris notes in !5514 that Windows POSIX-like read/write/socket APIs return int. Merged !5520 introduces ws_file_size_t and ws_file_ssize_t so project wrappers match the actual Windows and POSIX APIs. Use the exact ABI type at a wrapper boundary and make conversions explicit.

## Semantic API variants
Merged !5515 splits NUL-terminated regex matching from explicit-length matching and removes SIZE_MAX as a magic mode sentinel. Prefer separate semantic APIs over stealing a valid numeric value for an out-of-band mode.

## Runtime-only sensitive preferences
Merged !5519 adds PREF_PASSWORD: the value is usable during the process lifetime but is not written to or restored from the preferences file. Test both runtime reuse and restart/persistence behavior.

## Empty versus default
Merged !5538 preserves empty extcap values and adds an explicit reset-to-default action. Do not overload an in-domain empty value to mean unset/default.

## Package header source of truth
Merged !5532 makes Debian packages consume the build system's installed public-header tree instead of duplicating source header lists. Packaging should derive public/private inventory from the authoritative install/export rules.

## Shared error handling
John Thacker's !5548, !5549, and !5555 move text2pcap/text_import onto common report callbacks and explicit import status returns. Reusable GUI/CLI logic should report through the project layer and return status rather than terminate the process itself.

## Imported specification source
In !5512 Martin Mathieson and Pascal Quantin require removal of cosmetic edits to ASN.1 files extracted from specifications because regeneration will overwrite them. Preserve externally sourced specification text and change the authoritative source or generator instead.

## Checker source classes
!5512 exposes a false positive on packet-PROTOABBREV.c and !5537 adds a named exclusion. Later !5663 generalizes the issue to template-source classes. Prefer a semantic source-class rule over growing one-off exceptions.

## Public proto-tree APIs
Merged !5524 replaces an internal proto-tree API call with a registered field through public APIs and marks the synthetic value generated. User-visible synthetic data should use registered fields, not internal tree helpers.

## Configuration-dependent framing
Merged !5521 derives IEC101 fixed-frame PDU length from the configured link-address width instead of hard-coding the common case. Framing callbacks must honor the active protocol domain.

## Identifier namespaces
Merged !5530 renames pcap_link_type to wtap_encap_type because the value is a Wiretap encapsulation identifier. Name values for the namespace they actually belong to.

## Workflow corroboration
Anders Broman asks for a single squashed commit in !5559. Closed !5513 is remade after commit-message feedback and superseded by merged !5539. Treat these as workflow corroboration, not implementation precedent.
