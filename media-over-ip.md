# Media over IP / SMPTE Notes

## Current focus

Recent work has centered on SMPTE ST 2110-40, SMPTE ST 2038, ancillary data, and MPEG PES integration.

## ST 2110-40

- Validation work uses known-good captures, including NEO captures.
- Fuzzing support has been brought up with `fuzzshark_st2110_40`.
- When reviewing or extending the dissector, compare behavior with other RTP payload dissectors and with ANC parsing elsewhere in Wireshark.

## ST 2038

- ST 2038 work depends on MPEG PES dispatch/integration so the dissector can actually be reached from PES payloads.
- A previously generated `packet-st2038.c` build exposed unused local variables (`c_not_y` and `horizontal_offset`), a reminder to keep parser scaffolding warning-clean.
- ANC semantic identification must account for DID/SDID combinations. In particular, do not present an SDID value by itself as uniquely identifying ATC if the definition depends on both DID and SDID.

## VANC discovery during capture analysis

- Treat every capture-analysis task as an opportunity to inventory VANC content, regardless of whether the capture is ST 2110-40, ST 2038, MPEG-TS, SDI-derived, or another encapsulation carrying SMPTE ST 291 ancillary data.
- If a VANC type is encountered that does not already have a corresponding payload dissector implemented in `packet-smpte-291-vanc.c`, call it out explicitly to Devin even when it is incidental to the task being performed.
- Maintain `vanc-sample-index.md` as the cumulative lookup table of observed VANC types and the capture filenames containing them. Record both already-supported and unsupported types so it can serve as a future test-vector/sample locator.
- Identify a type from the complete applicable identifier tuple, especially DID/SDID. Do not infer a semantic type from SDID alone where DID also participates in the definition.
- If a payload is not confidently identified, record the numeric DID/SDID and mark the semantic identification as unknown or unconfirmed rather than guessing.

## Test vectors

- `libklvanc` contains vectors familiar to the developer, but independent known-good sources are desirable for validation.
- NEO captures are already in use for ST 2110-40 validation; continue seeking additional independent sources, including vendor/interoperability material where legally/publicly available.
- The VANC sample index is intended to make capture reuse practical: before searching externally for a VANC test vector, consult `vanc-sample-index.md` for previously encountered captures containing that type.

## Future work

Potential related areas include ST 2110-43 and broader ancillary-data dispatch/reuse. Before implementing, investigate whether existing ANC/ST 2038 code should be reused rather than duplicated.
