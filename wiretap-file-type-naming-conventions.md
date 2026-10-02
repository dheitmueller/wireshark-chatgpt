# Wiretap File-Type Naming Conventions

This file records durable conventions for distinguishing machine-readable capture-format identity from human-facing descriptions.

## Give a file format a stable machine name and a separate human description

Merged master MR !2080, authored by Guy Harris, makes the distinction explicit across Wiretap: the former “short name” becomes the file type **name**, used for lookup and command-line arguments, while the former display “name” becomes the **description**, intended for people. APIs and callers are renamed accordingly.

Guy-authored merged !2082 then removes a space from the `systemd journal` machine name because that identifier is accepted on command lines. Stable backports !2083/!2084 carry the same policy.

**Naming rule:** use a compact stable machine name for registry lookup, CLI arguments, and scripting. Keep descriptive prose in a separate human-facing field.

**Confidence:** Extremely high. Consecutive merged master API/naming changes authored by Guy Harris with stable backports.

## Prefer semantic name lookup over fixed numeric compatibility tables

Merged master !2076 exposes Wiretap file-type name/description and name-to-ID lookup to WSLua. Guy-authored !2088 explicitly deprecates the legacy numeric `wtap_filetypes` table for new scripts; !2089/!2090 backport the guidance. Merged !2092 completes the architectural reason: built-in file types are runtime registrations owned by their format modules.

**API rule:** resolve a known format by its semantic name at the point where a numeric runtime identity is needed. Treat legacy numeric tables as compatibility surfaces, not the model for new code.

**Confidence:** Extremely high. Merged master architecture and API changes authored by Guy Harris, with maintained-branch backports.
