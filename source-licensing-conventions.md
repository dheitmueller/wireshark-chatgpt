# Wireshark Source Licensing Conventions

This file records durable conventions for incorporating third-party source material into Wireshark. Current upstream source and project policy remain authoritative.

## Preserve compatible third-party licensing and express it in the project-standard SPDX form

Reusing implementation logic from another open-source project does not make the imported material license-free merely because the source license is compatible with Wireshark's. Preserve the applicable license/attribution information and express it using Wireshark's normal file-level licensing form rather than copying an ad hoc license block into the middle of a source file.

Merged master MR !20838 reused center-frequency calculation logic from `iw`. During review John Thacker identified the imported text as ISC-licensed, explicitly noted that ISC is compatible with Wireshark's GPL-2.0-or-later licensing, and requested that the license be handled the way Wireshark normally handles such material: at the top of the file using `SPDX-License-Identifier: ISC`. He also pointed to existing project source as the model to follow. The MR was updated and merged.

**Implementation rule:** when copying or adapting third-party implementation material, identify the source license before submission, preserve any required attribution, verify compatibility with Wireshark's licensing, and represent the imported license using the project's established SPDX/file-header convention. Do not bury a copied license notice next to the imported function or silently absorb externally licensed code into the surrounding file.

**Review rule:** source provenance is part of code review. When a contribution says it is adapted from another project, review both the implementation and the licensing/attribution treatment, and prefer an existing Wireshark example for formatting and placement.

**Confidence:** Very high. Direct review guidance from John Thacker on a merged master MR, followed by acceptance of the corrected form.