# External Registry Label Notes

Wireshark merge request 9384 refreshed the CIP vendor table from ODVA data. Review discussed names that were historically or stylistically surprising, including a company whose modern name differs from the ODVA and EDS name. The accepted direction was to keep the registry value so Wireshark remains aligned with the protocol authority and related tooling. Gilbert Ramirez likewise treated ODVA Reserved entries and unusual spelling as authoritative unless the registry itself changes. Gerald Combs confirmed that non-ASCII UTF-8 is acceptable in source comments and strings when used sparingly.

Durable convention: tables representing externally assigned registries preserve the authoritative registry's label, spelling, and reserved-state semantics rather than silently editorializing them. Deliberate local overrides or obsolete-entry compatibility layers are explicit.

Confidence: very high.
