# Durable conventions from MRs !108-!157

## Stream framing belongs to the byte-stream layer

Merged !123, with unusually authoritative discussion from Guy Harris and Peter Wu, establishes that QUIC STREAM data is an ordered byte stream and does not preserve application-message or QUIC-frame boundaries. A subdissector carried on QUIC must therefore determine its own PDU boundaries and request more data through the normal desegmentation contract when necessary. Existing SMB-over-TCP/NBSS framing logic should be reused rather than replaced with QUIC-frame assumptions.

## Choose field type from the protocol specification's semantic contract

Closed !127 is not implementation precedent, but Guy Harris's review is strong negative evidence. The TACACS+ Accounting Reply `data` field is described by the protocol drafts as text suitable for administrative display; arbitrary octets are reserved for fields specifically identified for protocol processing. A library or server implementation calling some related storage “octets” is not sufficient reason to register the Wireshark field as bytes.

## Preserve raw payload fields without duplicating visible tree clutter

Merged !121 always creates `rtp.payload` and hides it only when a registered payload dissector succeeds. This keeps raw payload bytes filterable and available to consumers while avoiding redundant visible output when a higher-level semantic decode exists.

## Scope CI to the files and artifacts reviewers need

Merged !111 uses the existing Wireshark development image instead of building another environment, triggers documentation work only when documentation or its WSLua source changes, and publishes the generated manuals as pipeline artifacts so reviewers can inspect rendered output. Merged !140 applies the same philosophy to static analysis: cover project-owned Qt code, but exclude third-party QCustomPlot sources explicitly instead of weakening checks globally.

## Keep generated sources reproducible

Merged !142 adds the NGAP template header to the CMake generator inputs while correcting the generated header. Master fixes !133 and !132 update both ASN.1 templates and generated S1AP/X2AP sources. Larger generated updates !145, !143, and !109 follow the same pattern. If a checked-in generated file changes, update the authoritative generator input/configuration in the same change and ensure the build describes that dependency.

## UTF-8 source is supported, but use non-ASCII deliberately

Merged !118 updates Wireshark's developer guidance to permit UTF-8 source. Non-ASCII characters should still be used sparingly because supported compilers, editors, and terminal environments do not all behave identically. Most Wireshark strings and console output are UTF-8; Qt's API uses UTF-16.

## Treat submission mechanics as part of maintainability

Pascal Quantin's !119 review explicitly requires a protocol/component prefix in the first commit-message line. !111 and !110 ask contributors to squash accumulated fixup commits into one logical change. !120, !117, and !108 reinforce enabling maintainer commits so core developers can perform rebases and minor corrections without a contributor round-trip. !128 additionally demonstrates that repository-layout changes must be reflected in repository-wide policy/checker configuration such as the license checker.
