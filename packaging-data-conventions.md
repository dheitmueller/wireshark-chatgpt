# Wireshark Packaged-Data Conventions

This file records durable conventions for resource files that must work both from the source/build tree and from installed Wireshark packages. Current upstream build and packaging definitions remain authoritative.

## Adding a runtime data resource requires auditing every installer/package path that ships its peers

A resource can work perfectly in a developer build and still be absent from an installed application when platform packaging uses explicit file manifests. When adding a new dictionary, profile, schema, plugin-side data file, or similar runtime resource, review both the runtime index/reference and the packaging definitions that enumerate neighboring files.

Merged master MR !14261 adds a Travelping Diameter dictionary. Martin Mathieson noticed during review that the new XML was referenced by the Diameter dictionary but was missing from the Windows NSIS file lists, based on where the peer `Metaswitch.xml` appeared. The author confirmed the omission and updated both `packaging/nsis/wireshark.nsi` and `packaging/nsis/logray.nsi`; Anders Broman then approved the merged result.

**Submission rule:** when adding an installed data file, search for a comparable existing peer and follow every place that peer is registered, staged, installed, packaged, or explicitly listed. Do not assume that adding the resource to the source-tree index or CMake inputs automatically makes every installer include it.

**Testing rule:** where practical, validate at least one installed/package layout rather than only an in-tree run. If the contributor cannot build a target platform, call that limitation out and use manifest review/CI to cover the platform-specific path.

**Confidence:** Very high. Merged master change with a concrete maintainer review finding; the packaging omission was corrected before Anders Broman approved the MR.

## RADIUS dictionary additions must update both the runtime index and explicit installers

Merged MR !8369 adds a 5x9 RADIUS dictionary. Alexis La Goutte immediately called out that adding the dictionary file and including it from the master RADIUS dictionary was not sufficient for Windows packaging; the contributor then added the new file to both NSIS manifests. Alexis also asked whether the MSI/WiX path needed a corresponding update, prompting an explicit check of that packaging path rather than an assumption.

**Submission rule:** when adding a packaged protocol dictionary or peer data file, update the runtime include/index and search every installer/package implementation for explicit manifests. Confirm which package systems gather the directory automatically and which enumerate files manually.

**Review rule:** use a comparable existing peer file to discover all packaging touch points; do not infer cross-platform packaging behavior from the source-tree layout.

**Confidence:** Very high. Merged master data-file addition with direct packaging review from Alexis La Goutte.


## RADIUS dictionary additions must update both the runtime index and explicit installers

Merged MR !8369 adds a 5x9 RADIUS dictionary. Alexis La Goutte called out that adding the dictionary file and including it from the master RADIUS dictionary was not sufficient for Windows packaging; the contributor then added the new file to both NSIS manifests. Alexis also asked whether the MSI/WiX path needed a corresponding update, prompting an explicit check of that packaging path.

**Submission rule:** when adding a packaged protocol dictionary or peer data file, update the runtime include/index and search every installer/package implementation for explicit manifests. Confirm which package systems gather the directory automatically and which enumerate files manually.

**Confidence:** Very high. Merged master data-file addition with direct packaging review from Alexis La Goutte.
