# Dissector Extension-Point Preservation Notes

Wireshark merge request 9368 refactored PFCP enterprise IE handling. Review caught that the first version removed the enterprise dissector table and therefore removed the ability for custom enterprise dissectors to register independently. The table was restored. Anders Broman stated the architectural direction that built-in vendor IE handling should work through the same registered-dispatch model used by external dissectors.

Durable convention: refactors of built-in vendor or profile handling preserve established dissector-table extension contracts. Built-in and external handlers use the same registration and dispatch boundary where practical, so an internal cleanup does not silently remove plugin or custom-dissection capability.

Confidence: extremely high.


## Consult known custom-dissector consumers before changing a shared dissector helper contract

Merged master MR !5135, authored by Pascal Quantin, changes the signature of `dissect_gtpv2_tai()` so callers can distinguish EPS and 5GS TAI layouts. Before finalizing the change, Pascal explicitly asked Anders Broman whether the signature change was acceptable for a known custom dissector and stated that he would add a new helper instead if it was not. Anders confirmed that his internal consumer tracks the development branch, after which the same fix was backported as !5137 and !5139.

**Compatibility rule:** a helper used by known external/custom dissectors is an extension contract even when it is not a formal stable ABI. Before changing its signature, identify known consumers and decide whether an additive helper is safer than an in-place break.

**Stable-branch rule:** compatibility deserves extra scrutiny when the same API-shape change is intended for maintained branches; do not assume an internal-tree call-site update proves all consumers are covered.

**Confidence:** Very high. Merged master protocol-expert change with explicit compatibility consultation and two accepted stable backports.
