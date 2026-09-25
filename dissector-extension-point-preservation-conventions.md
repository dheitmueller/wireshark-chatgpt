# Dissector Extension-Point Preservation Notes

Wireshark merge request 9368 refactored PFCP enterprise IE handling. Review caught that the first version removed the enterprise dissector table and therefore removed the ability for custom enterprise dissectors to register independently. The table was restored. Anders Broman stated the architectural direction that built-in vendor IE handling should work through the same registered-dispatch model used by external dissectors.

Durable convention: refactors of built-in vendor or profile handling preserve established dissector-table extension contracts. Built-in and external handlers use the same registration and dispatch boundary where practical, so an internal cleanup does not silently remove plugin or custom-dissection capability.

Confidence: extremely high.
