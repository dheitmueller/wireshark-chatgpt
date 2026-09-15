# Automated Wireshark MR review: !26463–!26512

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection method: rebuilt the already-reviewed set from `reviewed-mrs.md` and all available `reviewed-mrs-automation/` ledgers, preserving the historical !17571–!17620 batch. Selected the 50 highest-numbered corpus MRs not present in that set. This run corrects the prior descending frontier: the corpus contains newer MRs above the previously reviewed !26365–!26393 batch, so those higher unreviewed MRs take priority over continuing below !19290.

Reviewed MRs (exact set, descending):

!26512, !26511, !26510, !26509, !26508, !26507, !26506, !26505, !26504, !26503,
!26502, !26501, !26500, !26499, !26498, !26497, !26496, !26495, !26494, !26493,
!26492, !26491, !26490, !26489, !26488, !26487, !26486, !26485, !26484, !26483,
!26482, !26481, !26480, !26479, !26478, !26477, !26476, !26475, !26474, !26473,
!26472, !26471, !26470, !26469, !26468, !26467, !26466, !26465, !26464, !26463.

Count: 50.

Review notes: merged work was weighted above closed/superseded work; maintainer review comments were weighted by authority. Strong examples included !26511 (Bluetooth ACL connection-handle dissector table, clean extension point that bypasses standard fragmentation/L2CAP only on a successful vendor-specific match), !26466 (IEEE 11073 SFLOAT/FLOAT `is_nan` semantics integrated into the ftype contract so display-filter semantic validation can reject invalid ordered comparisons), !26467 (missing HTTP/3 control-frame dissection), and !26483 (removal of obsolete X11 lookup code superseded by `value_string_ext`). These primarily corroborate existing notebook conventions on dissector extension points, shared type-semantic contracts, protocol completeness, and removal of obsolete parallel mechanisms; no new durable convention was added solely to avoid duplicating established guidance.
