# Wireshark Type-Domain Conventions

## Equal numeric values do not make independent semantic domains interchangeable

Merged master MR !7915, authored by Guy Harris, fixes MS Proxy state that stored packet-type `PT_*` values and cast them to `endpoint_type`. TCP and UDP happened to use the same numeric values in both enums, but that coincidence was not a valid contract. The accepted code stores `endpoint_type` directly and assigns `ENDPOINT_*` values.

**Rule:** when two enums or identifier spaces represent different concepts, keep the declared type and constants from the domain the API expects. Do not type-pun or cast between domains solely because current integer values happen to coincide.

Release-4.0 MR !7916 carries the same fix.
