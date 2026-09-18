# Wireshark MR review automation ledger: !16596-!16608

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest previously-unreviewed MR toward older MRs. Before selection, the existing `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md` were consulted. The previously reviewed !17571-!17620 batch remains part of the already-reviewed set. The preceding ledger ended at !16609. The corpus snapshot begins at !16596, so only 13 previously-unreviewed MRs remain below !16609; this run therefore reviews 13 rather than manufacturing a 50-MR range that does not exist in the corpus.

## Exact reviewed set

!16608, !16607, !16606, !16605, !16604, !16603, !16602, !16601, !16600, !16599, !16598, !16597, !16596

## Review notes

- **!16608 — merged, master, Guy Harris.** `pidl` portability fix: when the compiler is Clang-based, use `cc -E` rather than `cpp` because at least Xcode 15.2's `cpp` does not strip `//` comments while the compiler-driver preprocessing path does. High-authority implementation evidence, but tool-specific and not broad enough to add a new notebook rule.
- **!16607 — merged, master, Gerald Combs.** GitLab CI interruptibility adjustment. Useful CI maintenance context; no durable coding/dissector convention extracted.
- **!16606 — merged, master.** Adds SBAS L1 MT18 dissection. Anders Broman specifically requested using `proto_tree_add_bitmask()` in place of repeated/manual packed-field handling and suggested applying it throughout the code. This strongly corroborates the existing notebook guidance to prefer Wireshark's protocol-tree/bitfield APIs over local extraction/rendering patterns; no duplicate rule added.
- **!16605 — merged, master.** USB high-bandwidth endpoint sanitization and USBLL interrupt-transfer reassembly fixes. The MR deliberately retained two semantically separate commits despite an automated suggestion to squash trivial commits; this is useful evidence that the project's squash guidance is about trivial/history-noise commits, not a mandate to collapse logically independent changes. The implementation also carefully derives high-bandwidth payload capacity from wMaxPacketSize and corrects transfer-end/reassembly state for exact Max Packet Size multiples. Existing state/reassembly guidance already covers the durable implementation lesson.
- **!16604 — merged, master, Gerald Combs.** Updates documentation CI paths after the guide sources moved from `docbook/` to `doc/`. Maintenance-only; no new convention.
- **!16603 — merged, master, Anders Broman.** Removes unnecessary direct GLib includes from a broad set of headers/files. Corroborates dependency/include hygiene but adds no distinct rule beyond keeping dependencies minimal and using project abstractions.
- **!16602 — merged, master.** Fixes a Bluetooth display buffer whose fixed size became too small after terminology changed from `Slave` to longer `Peripheral`; author tested before/after against the same capture. Useful reminder that user-visible terminology changes can invalidate fixed presentation-size assumptions. No new rule added because current code should avoid fragile fixed string sizing where practical.
- **!16601 — merged, master.** User's Guide update adding FTP-DATA to Export Objects documentation. Documentation-only; no new convention.
- **!16600 — merged, master, Anders Broman.** Replaces PFCP `g_alloca()` allocation used for registration-time IE references with `wmem_alloc(wmem_epan_scope(), ...)`. This supports using Wireshark's allocator/lifetime model rather than ad-hoc GLib allocation mechanisms where the data has an epan lifetime. Existing ownership/lifetime guidance is sufficient.
- **!16599 — merged, master, Anders Broman.** Converts remaining DCE/RPC dissectors from GLib integer aliases (`gint`, `guint16`, etc.) to C99/stdint types. Strong corroboration of the project's migration toward standard C types; already represented by later notebook evidence.
- **!16598 — merged, master, Alexis La Goutte.** Corrects a CIGI header-field display label from `Symbol Surface Definition` to `Symbol Clone` while retaining the proper `cigi.symbol_clone` filter. Small correctness fix; no broader new rule.
- **!16597 — merged, master, Guy Harris.** Removes obsolete DCE/RPC `guint1632` / `guint3264` typedefs in favor of `uint1632_t` / `uint3264_t`. High-authority continuation of the standard-type cleanup, reinforcing rather than extending the notebook's current type guidance.
- **!16596 — merged, master.** Zigbee security processing now continues for zero-length payloads so APS ACK frames still expose the link key/security information. Durable semantic point: an empty application payload does not imply there is no meaningful protocol/security metadata to process. This is protocol-specific enough that no general notebook rule was added.

## Durable conclusions

This final partial corpus batch mostly reinforces conventions already captured elsewhere: prefer Wireshark-native packed-field helpers, use standard C types instead of legacy GLib aliases, respect allocator/lifetime semantics, preserve logically separate commits even when trivial commits should be squashed, and do not let zero payload length suppress meaningful header/security processing. No convention file was changed because adding duplicate formulations would make the notebook less useful.

## Continuation

There is no lower-numbered MR in corpus commit `a3b98766b09f6822e4f2d38d2925205c0ee92383`: the snapshot starts at !16596. Future review should rebuild the already-reviewed set and check for a newer corpus commit before selecting another batch. Do not assume that numeric gaps outside this corpus snapshot are reviewed.