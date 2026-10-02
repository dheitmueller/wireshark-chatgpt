# Wireshark Ownership and Copy Conventions

This file records durable ownership/copying conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Stored container entries must own data that survives the caller

When a persistent container copies a struct that contains pointer members, copying the struct itself does not transfer or duplicate the pointed-to storage. If the container's destroy callback later frees those members, each stored entry must own an independent allocation with a lifetime at least as long as the container entry.

Merged master MR !25616 fixes recent-file window geometry state where a hash-table replacement could double-free a duplicated geometry string. The accepted implementation makes `window_geom_save()` duplicate `qt_geom` into the stored object, so replacement/destruction owns exactly the copy it frees. The Qt caller also keeps the temporary `QByteArray` returned by `saveGeometry().toHex()` alive in a named local until `window_geom_save()` has copied its `constData()` pointer. Stable-branch backports !25620 and !25621 preserve the same ownership arrangement.

**Implementation rule:** for container-owned structs with pointer members, define ownership per member rather than assuming a shallow struct copy is sufficient. Duplicate borrowed data at the ownership-transfer boundary, ensure temporary backing objects remain alive until that copy completes, and make destroy callbacks free only storage the stored object actually owns.

**Confidence:** Very high. Merged master lifetime/double-free fix by John Thacker with accepted release-4.6 and release-4.4 backports.

## Use shallow copies for transient lookup and deep copies at the persistence boundary

A temporary object used only to compare or look up packet-local state does not need to duplicate every referenced address or string. Conversely, an object inserted into persistent tap, conversation, or UI state must own data whose lifetime outlasts the packet.

Merged master MR !9921, authored by John Thacker, adds `rtpstream_id_copy_pinfo_shallow()` for the transient RTP stream ID used only while examining existing entries. The helper uses shallow address copies and explicitly documents that callers must not pass such an ID to `rtpstream_id_free()`. When a new stream entry is actually created, the existing deep-copy helper is still used so the persistent entry owns its addresses.

**Implementation rule:** choose shallow versus deep copy from the destination lifetime. Borrow packet-owned members for a short-lived comparison key, but deep-copy them at the boundary where state becomes persistent. Keep destruction semantics paired with the copy operation; a borrowed shallow copy must not be freed as though it owned its pointees.

**Confidence:** Very high. Merged master ownership fix authored by John Thacker with the shallow/deep distinction documented in the helper comments and call sites.

