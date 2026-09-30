# Review findings: Wireshark MRs !3461–!3510

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

The exact 50-MR membership is in `ledger-3461-3510.md`. Merged work is weighted above closed work; direct maintainer guidance is weighted accordingly.

## Deep / discussion-focused findings

- **!3510 (merged):** `wslog` modernizes time acquisition. Guy Harris raised common-helper reuse; the accepted design preserves actual subsecond resolution rather than claiming precision a fallback does not provide.
- **!3508 (merged, Guy Harris):** pcapng comment/custom option writing moves into common code, leaving block-specific option behavior to callbacks.
- **!3498 (merged):** Nordic BLE timing history is keyed by capture interface ID, preventing state from one interface corrupting another.
- **!3495 (merged):** OSPF validates sub-TLV length before subtracting its 4-byte header. Jaap Keuter objected that a companion tvbuff assertion rejected a valid zero-length input; assertion predicates must match the complete valid API domain. Alexis La Goutte also requested unrelated work be split.
- **!3492 (merged):** S101 changes `COL_PROTOCOL` only after recognizing an S101 header, avoiding false claims on unrelated TCP/9000 traffic.
- **!3491 (merged, Guy Harris):** formatting `timeval.tv_usec` must not assume a platform-specific integer width.
- **!3490 (merged, John Thacker):** MP2T reassembly uses stream identity (conversation plus direction), not packet addresses/ports that nested payload dissectors may modify. Supplied captures demonstrate the failure and fix.
- **!3489 (merged):** the log writer stops calling GLib date-time routines because those routines can themselves log and recursively re-enter or abort the Wireshark log handler.
- **!3488 (merged, Guy Harris):** Wiretap option-iteration callbacks return success/failure so I/O errors stop iteration and propagate directly.
- **!3484 (merged, Guy Harris):** pcapng end-of-options writing is centralized in one helper.
- **!3480 (merged):** ESP alignment fix with a reproducer. Pascal Quantin explicitly recommends expert info over plain appended error text because it flags the packet and is filterable; the broader expert cleanup was deferred.
- **!3478 (merged):** NetPerfMeter composes protocol-column text by appending. Pascal Quantin requires the unrelated S101 fix to move to its own MR and records the poor commit-history result of relying on merge-time stash/squash handling.
- **!3477 (merged, Pascal Quantin):** packet-pool memory must be released with `wmem_free(pinfo->pool, ...)`, not `g_free()`; packet-scope teardown already owns eventual reclamation.
- **!3476 (merged):** logging and command-error initialization happens early enough that startup failures and dumpcap capture-child messages use the correct channel/format.
- **!3475 (merged):** `config.h` is removed from a shared header and included by the source that needs it.
- **!3473 (merged, discussion-focused):** Gerald Combs removes obsolete classful-network/IPX examples from filter documentation.
- **!3472 (merged):** João Valverde minimizes `ws_assert`/logging dependencies to reduce include-order side effects and recursive logging risk; C99 `__func__` is accepted as portable.
- **!3464 (merged):** `codecs.h` takes its direct GLib dependency instead of depending upward on `epan/epan.h`.
- **!3463 (merged, John Thacker):** use `fragment_get()` only on first pass and `fragment_get_reassembled_id()` on redissection; also do not free a TVB owned by reassembly.
- **!3462 (merged):** Follow QUIC Stream tracks connection-local stream IDs; Alexis La Goutte requests generic comparison support be moved to common wmem utility code.
- **!3461 (merged, John Thacker):** MP2T analysis state is keyed by conversation and direction so opposite-direction streams do not share continuity or fragment-ID history.

## Scanned, no additional durable convention

!3509, !3507, !3506, !3505, !3504, !3503, !3502, !3501, !3500, !3499, !3497, !3496, !3494, !3493, !3487, !3486, !3485, !3483, !3482, !3479, !3474, !3471, !3470, !3469, !3468, !3467, !3466, and !3465 were examined for their final diffs and available discussion. They were straightforward backports, protocol-specific fixes/features, generated/data maintenance, or already-covered patterns.

**!3481** was closed/unmerged without substantive human review and is retained only as historical context, not implementation precedent.

No SMPTE 291/VANC packet type was encountered in this batch.
