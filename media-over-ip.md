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

## Test vectors

- `libklvanc` contains vectors familiar to the developer, but independent known-good sources are desirable for validation.
- NEO captures are already in use for ST 2110-40 validation; continue seeking additional independent sources, including vendor/interoperability material where legally/publicly available.

## Future work

Potential related areas include ST 2110-43 and broader ancillary-data dispatch/reuse. Before implementing, investigate whether existing ANC/ST 2038 code should be reused rather than duplicated.
