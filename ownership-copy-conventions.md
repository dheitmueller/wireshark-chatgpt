# Wireshark Ownership and Copy Conventions

This file records durable ownership/copying conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Stored container entries must own data that survives the caller

When a persistent container copies a struct that contains pointer members, copying the struct itself does not transfer or duplicate the pointed-to storage. If the container's destroy callback later frees those members, each stored entry must own an independent allocation with a lifetime at least as long as the container entry.

Merged master MR !25616 fixes recent-file window geometry state where a hash-table replacement could double-free a duplicated geometry string. The accepted implementation makes `window_geom_save()` duplicate `qt_geom` into the stored object, so replacement/destruction owns exactly the copy it frees. The Qt caller also keeps the temporary `QByteArray` returned by `saveGeometry().toHex()` alive in a named local until `window_geom_save()` has copied its `constData()` pointer. Stable-branch backports !25620 and !25621 preserve the same ownership arrangement.

**Implementation rule:** for container-owned structs with pointer members, define ownership per member rather than assuming a shallow struct copy is sufficient. Duplicate borrowed data at the ownership-transfer boundary, ensure temporary backing objects remain alive until that copy completes, and make destroy callbacks free only storage the stored object actually owns.

**Confidence:** Very high. Merged master lifetime/double-free fix by John Thacker with accepted release-4.6 and release-4.4 backports.

## Treat block-option getter results as borrowed unless ownership is explicitly transferred

Merged !3699, authored by Guy Harris, fixes ERF code that retained a string returned by `wtap_block_get_nth_string_option_value()` and later freed it. The returned pointer refers to storage owned by the block, so retaining that pointer across block lifetime changes can leave a dangling alias and freeing it independently can double-free storage. The accepted fix takes a `g_strdup()` copy before storing the value in independently-owned writer state.

**Implementation rule:** assume values returned by Wiretap block-option getters are borrowed from the block unless the API contract explicitly transfers ownership. If the receiver needs an independent lifetime or will free the value, copy it at that boundary.

**Review rule:** for pointer-returning getters, verify both lifetime and destruction responsibility. A getter that exposes internal storage is not a transfer-of-ownership API merely because the returned type is mutable.

**Confidence:** Extremely high. Guy Harris-authored merged correctness fix.
