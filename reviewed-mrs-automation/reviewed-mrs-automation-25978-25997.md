# Supplemental Reviewed Wireshark Merge Requests — !25978–!25997

This is a continuation of the reviewed-MR ledger. Consult this file together with `reviewed-mrs.md`, `reviewed-mrs-automation.md`, and earlier continuation ledgers when selecting unreviewed MRs.

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit used for this batch: `a67598e5dd77b04de676ab8cee5e2754525edf8e`

Exactly twenty MRs were reviewed in this batch: **!25978 through !25997 inclusive**.

| MR | Review depth | Outcome / durable relevance |
|---|---|---|
| !25978 | Deep | Merged John Thacker LBMR bounds fix. Creates a subset tvb using the protocol-declared TMR length and performs nested parsing relative to that bounded view. Promoted as a parser rule: when an element has an explicit length, constrain its subparser to that element rather than relying on outer-tvbuff bounds. |
| !25979 | Discussion-focused | Open CoAP presentation change to use RFC 7959 block notation. Stig Bjørlykke caught a `Bock`/`Block` presentation issue and the thread remained unresolved in this corpus snapshot. Down-weighted because the MR is still open. |
| !25980 | Scanned | Merged Couchbase DCP Cache Transfer opcode dissection, approved by Anders Broman and merged by John Thacker. Useful protocol extension, but no new general convention beyond existing opcode/subdissector patterns. |
| !25981 | Scanned | Merged release-4.6 backport of the Bluetooth HFP `+XAPL` minimum-length fix. Corroborates the existing fixed-format-text rule from !25976. |
| !25982 | Discussion-focused | Closed draft experiment replacing Wireshark's Lua packaging with vanilla Lua; superseded by !26035. Packaging provenance and vcpkg compatibility were discussed, but the design was not accepted here and was deliberately not promoted. |
| !25983 | Scanned | Merged release-4.4 backport of the same HFP `+XAPL` minimum-length fix. Corroboration only. |
| !25984 | Deep | Merged John Thacker Bluetooth BR/EDR fix registering a reassembly table that was used without registration. Maintainers immediately discussed detecting this structural mistake automatically. |
| !25985 | Deep | Merged UET big-endian CRC fix. Removes redundant CRC byte swapping/`htonl()` after the CRC32C API semantics were misread. Promoted as a portability rule: checksum/helper APIs have explicit byte-order contracts; do not layer host/network conversions on them without tracing those contracts. |
| !25986 | Deep | Merged Martin Mathieson checker enhancement following !25984. `check_typed_item_calls.py` now detects unregistered reassembly tables. Strong evidence for converting recurring structural review defects into repository-enforced static checks. |
| !25987 | Scanned | Merged release-4.6 backport of the BR/EDR reassembly-table registration fix. Corroborates !25984. |
| !25988 | Scanned | Merged release-4.4 backport of the BR/EDR reassembly-table registration fix. Corroborates !25984. |
| !25989 | Deep | Merged new UDX transport dissector. Stateful analysis is computed on first pass and cached per packet; tests cover loss/reorder/duplication, wraparound, stream pairing, retransmission classes and Follow Stream byte identity. Reviewer Jaap Keuter also required a conservative stance toward a weak heuristic because of false-positive risk. Promoted into stateful-testing/heuristic guidance. |
| !25990 | Scanned | Merged broad Python import-order cleanup driven by Ruff/isort. Primarily tooling hygiene; no new convention beyond treating project lint configuration as authoritative. |
| !25991 | Discussion-focused | Merged additional Ruff cleanup. Martin Mathieson noted his local Ruff version differed from the project's version; John Thacker approved after CI-clean revisions. Corroborates the existing project-CI-toolchain rule. |
| !25992 | Deep | Merged John Thacker wiretap `log3gpp` bounds fix. Corrects pairwise hex decoding from an inclusive loop to a bound that only processes complete character pairs. Promoted as a parser/writer loop rule: bounds must guarantee every multi-byte/pairwise iteration has all bytes it reads. |
| !25993 | Deep | Merged John Thacker platform-policy change raising the minimum Windows version so UTF-8 ActiveCodePage can be relied upon. The MR was intentionally held as draft until consensus and required coordinated updater/website behavior plus a supported old-release path. Promoted as a platform-compatibility/submission rule. |
| !25994 | Deep | Merged John Thacker `phtoleu16` memory-safety fix. A helper that writes 16 bits accepted a 32-bit value on one memcpy-based path, allowing `sizeof(v)`-driven overread/overwrite behavior. Promoted as an API type-width rule. |
| !25995 | Scanned | Merged release-4.6 backport of the `log3gpp` pairwise bounds fix. Corroborates !25992. |
| !25996 | Deep | Merged John Thacker Qt software-update fix. WinSparkle can call back from another thread, so QTimer/QObject work is queued onto the owning Qt thread with `QMetaObject::invokeMethod(..., Qt::QueuedConnection)`. Promoted as a GUI/thread-affinity rule. |
| !25997 | Scanned | Merged release-4.6 backport of the `phtoleu16` width fix, including documentation that the destination must provide two bytes. Corroborates !25994. |

## Notebook changes from this batch

Durable conventions extracted from this batch:

- Bound explicit-length nested structures with a subset tvb before parsing their interior (!25978).
- Treat reassembly-table registration as a structural invariant and rely on the repository checker added in !25986 to catch missing registration (!25984/!25986).
- Respect checksum/helper byte-order contracts and test portability assumptions on architectures whose endianness exposes them (!25985).
- For stateful transport dissectors, make first-pass analysis stable across redissection, use conservative heuristics, and test realistic loss/reorder/duplication/wraparound and byte-identical follow reassembly (!25989).
- Pairwise decoders must bound loops so each iteration has a complete input unit (!25992/!25995).
- Platform minimum-version changes require project consensus and coordinated distribution/updater compatibility handling, not just compile-time changes (!25993).
- Width-specific helper APIs must use width-specific value types all the way through compiler-specific/memcpy paths (!25994/!25997).
- Marshal external/native callbacks to the owning Qt thread before operating on thread-affine QObject/QTimer state (!25996).

Open !25979 and superseded draft !25982 were deliberately down-weighted and were not treated as accepted architecture.