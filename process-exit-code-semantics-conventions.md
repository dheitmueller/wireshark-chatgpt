# Process Exit-Code Semantics Notes

Wireshark merge request 9367, authored and merged by Martin Mathieson, replaced a generic status value shared by unrelated failures with distinct values for invalid interfaces, files, filters, capabilities, initialization, and open failures. The same change updated dftest and the Python test suite to use the shared semantic vocabulary.

Durable convention: command-line programs expose distinguishable process results for meaningfully different failure classes, and tests assert those semantic results rather than relying on one generic nonzero value or scattered raw numbers.

Confidence: very high.
