# Export Object Size Conventions

MR 3271 changes the Export Object payload length to size_t. This follows Guy Harris review in MR 3268: an object stored in process memory is bounded by the addressable size domain, even when a protocol can describe a wider length.

Rule: use size_t for the size of a resident object or buffer. Keep wider protocol lengths separate, and check that they fit before converting them to a resident-object size.

Confidence: extremely high. Direct Guy Harris review was followed by the merged implementation.

## Earlier shared-writer evidence

Merged master MR !3260 introduced the shared `write_file_binary_mode()` helper with a `size_t` content length and bounded write chunks at the platform I/O boundary. Its Windows x86 build exposed the remaining Export Object `gint64` mismatch, and Guy Harris explained in the MR that an already resident buffer cannot exceed the `size_t` domain. The later !3268 review and merged !3271 change remain the authoritative Export Object API correction.

**Rule:** use the address-space-sized domain for resident buffers and adapt to narrower platform write-count types at the I/O boundary.

**Confidence:** Extremely high. Direct Guy Harris explanation corroborated by the shared writer and later merged type correction.
