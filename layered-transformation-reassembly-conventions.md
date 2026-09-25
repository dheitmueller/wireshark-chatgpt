# Layered Transformation and Reassembly Conventions

## A transformed byte stream can require its own reassembly layer

Merged master MR !10545, authored by John Thacker, fixes MySQL compressed-protocol handling. A complete TCP-level compressed packet can decompress into only part of one MySQL PDU, or into several MySQL PDUs. The accepted implementation therefore performs outer transport PDU handling and independent streaming reassembly over the decompressed byte stream.

**Implementation rule:** completing transport-layer reassembly does not prove that a generated/transformed payload contains complete application PDUs. Decompression, decryption, unchunking, tunneling, or similar transformations can introduce a new stream with its own framing boundary; model and reassemble that layer explicitly when the protocol permits fragmentation there.

**Review rule:** for transformed payloads, ask independently whether the outer container is complete and whether the resulting inner byte stream ends on an application-PDU boundary. Test both partial-inner-PDU and multiple-inner-PDU cases.

**Confidence:** Very high. Merged master reassembly fix authored by John Thacker and reviewed with the MySQL protocol maintainer.
