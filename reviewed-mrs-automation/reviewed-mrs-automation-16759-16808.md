# Wireshark MR review automation: !16759-!16808

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest available unreviewed MRs. Existing `reviewed-mrs.md` and all available files under `reviewed-mrs-automation/` were consulted before selecting this batch. The historical !17571-!17620 batch remains part of the already-reviewed set.

## Exact reviewed set

!16808, !16807, !16806, !16805, !16804, !16803, !16802, !16801, !16800, !16799, !16798, !16797, !16796, !16795, !16794, !16793, !16792, !16791, !16790, !16789, !16788, !16787, !16786, !16785, !16784, !16783, !16782, !16781, !16780, !16779, !16778, !16777, !16776, !16775, !16774, !16773, !16772, !16771, !16770, !16769, !16768, !16767, !16766, !16765, !16764, !16763, !16762, !16761, !16760, !16759

Count: **50**.

## Weighting and durable findings

Merged master MRs were treated as the strongest implementation evidence. Release backports, mechanical updates, documentation-only changes, and closed/draft/superseded submissions were counted but given less independent architectural weight.

- **!16807 (merged, John Thacker):** Qt idle dissection must not run while the capture-file read lock is held. The bug manifested while a large capture was still being idle-dissected and simultaneously saved into a new compression/file format, producing apparently corrupt-file errors. Durable lesson: lock scope around background/idle dissection is part of file-operation correctness; avoid retaining a read lock across work that can race with save/rewrite operations.
- **!16806 (merged):** adds LZ4 writing after independent-frame LZ4 fast seek support was established. This reinforces keeping wiretap compression read/write capabilities aligned with the seek model rather than treating compression as only a stream wrapper detail.
- **!16805 (merged):** Qt translation cleanup uses `QObject::tr` where translation is needed without forcing a `Q_OBJECT` macro solely for translation support. This is a Qt-specific implementation detail rather than a broad Wireshark convention.
- **!16804 (merged):** Python invalid-escape warnings are fixed by using raw strings where the source text is intended literally. This is compiler/interpreter hygiene and does not justify a new notebook rule.
- **!16799 (merged):** substantial Syslog enhancement adds TCP handling, RFC 6587 octet-counting framing, Syslog-over-TLS, and RFC 5424/3164 corrections. Useful corroboration that stream protocols need explicit framing semantics rather than assuming packet boundaries.
- **!16789 (merged, John Thacker):** documents why wiretap fast-seek state can include decompressed data. Primarily explanatory maintenance; no independent convention extracted.
- **!16779 (merged):** restores the required `errno.h` dependency after a prior cleanup exposed a build failure. Reinforces validating include cleanup across actual build configurations rather than assuming transitive declarations.
- **!16769 (closed draft):** Mellanox NCSI OEM temperature command support was closed unmerged and therefore was not used as an accepted implementation exemplar.
- **!16759 (merged, Guy Harris):** replaces a dissector-private MHz `unit_name_string` with Wireshark's standard `units_mhz`, accepting the standardized presentation so the dissector behaves like the rest of the application. Martin Mathieson suggested that a checker analogous to `tools/check_tfs.py` could detect private unit strings that duplicate standard ones. High-confidence durable lesson: built-in dissectors should reuse standard shared unit-name strings rather than defining local equivalents merely to preserve cosmetic spacing; recurring duplicate-declaration patterns are candidates for static checking.

## Notebook impact

No separate convention file was changed in this run. The strongest reusable observations reinforce conventions already represented in the notebook: prefer shared project helpers/data over local equivalents, treat stream framing explicitly, keep lock/state lifetime aligned with the operation it protects, and turn mechanically detectable repeated mistakes into static checks where worthwhile. The Guy Harris-authored !16759 change is retained here as high-authority evidence for future consolidation of shared-unit guidance.

## Continuation

Rebuild the reviewed-MR set from the ledgers before the next run. If no higher-numbered gap is discovered, the next descending candidate is **!16758**.
