# Wireshark Protocol Value Provenance Conventions

Merged MRs !9861 and !9862, both authored by Guy Harris, correct a Gryphon response field whose value comes from the corresponding request. The accepted code gives that response item a zero offset and zero length so it does not highlight unrelated response bytes, and changes the registered type from FT_UINT8 to FT_UINT32 to match the remembered value.

**Rule:** a value shown while dissecting one packet must not claim a byte range in that packet unless those bytes actually encode the value. Values obtained from transaction or conversation context should use source-less/generated presentation as appropriate to current Wireshark APIs, with a field type matching the semantic value.

**Confidence:** Extremely high. Direct Guy Harris-authored fixes on two merged release branches.
