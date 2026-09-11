# VANC Sample Index

This file is a cumulative index of SMPTE ST 291/VANC types encountered while reviewing capture files. Its purpose is to make known-good or at least real-world sample material easy to relocate for testing, validation, and future dissector development.

## Maintenance rules

- Update this file whenever any VANC type is encountered in a capture, regardless of capture/container/transport type and regardless of whether that VANC type is relevant to the immediate task.
- Include VANC types that already have Wireshark payload dissectors as well as types that do not.
- If an encountered VANC type does not already have a corresponding payload dissector implemented in `packet-smpte-291-vanc.c`, explicitly notify Devin during the capture review in addition to updating this file.
- Use the complete applicable identifier tuple for identification, normally DID/SDID. Do not identify a payload from SDID alone when DID also contributes to the definition.
- If the semantic type is uncertain, record the numeric DID/SDID and label the type as unknown/unconfirmed rather than guessing.
- Add newly encountered filenames to the existing type entry. Do not create duplicate entries for the same VANC type and do not remove prior filenames merely because a newer/better sample is found.
- Record filenames exactly as available to the user. Include a repository, directory, URL/source label, or other qualifier when known and useful for distinguishing or relocating the capture.
- The `Wireshark dissector` column describes whether the current `packet-smpte-291-vanc.c` implementation has a payload dissector for that type at the time the entry is updated. Re-check this status when working from a substantially newer Wireshark source tree.

## Encountered VANC types

| DID | SDID | Type / standard | Wireshark dissector | Capture files | Notes |
| --- | --- | --- | --- | --- | --- |

No capture filenames have been added yet. Populate this table opportunistically as captures are analyzed; do not reconstruct historical filenames from memory unless they can be verified.
