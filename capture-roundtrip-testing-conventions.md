# Wireshark Capture Round-Trip Testing Conventions

Merged master MR !5857, authored by John Thacker, changes text2pcap round-trip tests to use TShark's `--hexdump frames`. The previous generic hexdump could include decrypted or reassembled secondary data sources, which changed packet/data counts and weakened exact assertions.

**Testing convention:** a byte-preserving capture conversion test should explicitly export the original frame-byte domain. Derived data sources belong in separate tests whose contract intentionally covers transformed/reassembled/decrypted data.

**Review signal:** per-capture exceptions to byte or packet counts caused only by analyzer-generated secondary data sources indicate that the test is no longer isolating the converter's byte-preservation contract.

**Confidence:** Very high; merged master test correction authored by John Thacker.
