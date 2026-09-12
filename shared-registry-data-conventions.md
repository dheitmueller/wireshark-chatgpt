# Wireshark Shared Protocol Registry Data Conventions

This file records durable architectural conventions for protocol constants, registries, and other data shared across multiple dissectors and third-party plugins. Current upstream APIs remain authoritative.

## Do not make shared public registry data owned by an arbitrary consumer dissector

Protocol registry values that are consumed by unrelated dissectors are shared API/data, even if they are protocol-oriented rather than part of the generic dissection machinery. Moving them into whichever dissector happens to use them most visibly creates an artificial dependency and makes plugin reuse depend on that dissector being built.

Closed draft MR !22698 proposed moving the shared IANA address-family definitions into `packet-bgp.c`. João Valverde strongly rejected that direction: the values are useful to protocols unrelated to BGP, third-party plugins may need them, and choosing BGP merely because some owner had to be selected would make the architecture worse. The proposal was closed unmerged. The accepted successor, merged MR !22729, instead generates the IANA address-family and IP protocol registries into dedicated `packet-iana-data` source and declares `packet-iana-data.h` as a public dissector header. Guy Harris explicitly asked whether the header would remain public for third-party plugins and also corrected the conceptual description toward a shared “*shark dissection engine API,” rather than a Wireshark-only API.

**Architecture rule:** give registry data shared by otherwise independent dissectors an explicit shared owner and preserve public access when plugins may legitimately consume it. Do not hide common constants/value strings inside an arbitrary protocol dissector simply to reduce protocol-specific material in a lower layer. If authoritative registries can be generated from their standards body, prefer a dedicated generated data module over duplicated hand-maintained copies.

**Confidence:** Extremely high. A rejected draft documents the dependency failure mode; the merged replacement preserves shared/public ownership, with substantive review from João Valverde and Guy Harris and final merge by Michael Mann.
