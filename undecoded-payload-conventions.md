# Wireshark Undecoded Payload Conventions

## Keep unsupported payload bytes visible

During review of merged master MR !9865, the initial Matter dissector intentionally supported only part of the PDU. Jaap Keuter asked that the unimplemented remainder be passed to the data dissector rather than disappearing from view. The accepted revisions also keep encrypted header material visible as opaque bytes where appropriate.

**Rule:** when a dissector intentionally stops before the end of its TVB, hand the remainder to an appropriate subdissector or expose it as opaque bytes according to the protocol structure. An incomplete semantic decoder is acceptable when its scope is explicit; silently hidden bytes are not.

**Review:** check byte accounting through the end of the PDU for intentionally partial dissectors.

**Confidence:** Very high. Direct Jaap Keuter review on a merged new dissector, addressed before merge.
