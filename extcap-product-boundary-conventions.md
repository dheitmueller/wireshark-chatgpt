# Wireshark Extcap Product-Boundary Conventions

This file records durable conventions for deciding which Wireshark-family product owns an extcap. Current upstream packaging and product architecture remain authoritative.

## Classify an extcap by the semantic data it produces, not merely by the operating-system API it uses

An extcap's implementation technology or source subsystem does not by itself determine whether it belongs with Wireshark or Stratoshark. The important product boundary is the kind of capture data the extcap exposes to the user.

Merged master MR !23550 moved Windows system-event extcaps toward Stratoshark while retaining `etwdump` with Wireshark. During review Gerald Combs explicitly corrected the proposed move of `etwdump`: although it uses Windows ETW infrastructure, it emits network packets, so it belongs with Wireshark. The accepted MR was revised accordingly before merge.

**Implementation rule:** when assigning or moving an extcap between Wireshark-family products, classify it by the semantics of its emitted records. Network-packet capture sources belong with Wireshark even when their acquisition mechanism is also used for system-event tracing; event/log/system-activity sources may belong with Stratoshark. Audit packaging, installation, and frontend exposure against that semantic boundary.

**Confidence:** Very high. Merged master architecture/packaging change with direct Gerald Combs review that changed the proposed product assignment.

## Use extcap for platform-specific capture conversion that does not satisfy the Wiretap reader contract

A capture source can ultimately produce packets useful to Wireshark without belonging inside Wiretap's native file-reader abstraction. If the implementation is fundamentally a platform-specific conversion step, forcing it into Wiretap via filename-extension exceptions creates an API mismatch.

Merged master MR !468 began as ETL-to-pcapng conversion inside Wiretap. Dario Lombardo objected that Wiretap is a real file-reading library with regular open/read/seek semantics and that a conversion exception based on file suffix did not fit that contract. Graham Bloice agreed that extcap was a better boundary for the Windows-only conversion. The contributor reworked the MR into the merged `etwdump` extcap, and the final change also integrated the ETW dissector, Windows build guards, and both NSIS and WiX packaging. Guy Harris separately noted that a true native ETL reader requiring whole-file sorting would need a progress/lifecycle model for potentially expensive open-time work.

**Architecture rule:** if a source is best modeled as an external/platform-specific acquisition or conversion pipeline rather than as a regular random/sequential capture-file reader, use extcap rather than special-casing Wiretap. Add a Wiretap reader only when the format can honor Wiretap's normal reader contract or the core API is deliberately extended to support the required lifecycle.

**Packaging rule:** a platform-specific extcap must still be integrated coherently into the supported build and installer paths for that platform; do not let one installer silently lag the feature.

**Confidence:** Very high. The architecture changed in response to direct Dario Lombardo, Graham Bloice, and Guy Harris review, and the redesigned extcap implementation is the merged outcome.
