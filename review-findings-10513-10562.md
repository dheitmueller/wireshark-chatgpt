# Review findings: !10513–!10562

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

Primary durable findings from this 50-MR batch were promoted as follows:

- !10536 with precursor !10531: exception-safe persistent state; see `exception-safe-state-transition-conventions.md`.
- !10545: independent framing/reassembly after payload transformation; see `layered-transformation-reassembly-conventions.md`.
- !10513: stable filter identity and configuration-aware dynamic field registration; see `dynamic-filter-field-conventions.md`.
- !10542 and !10525: verify current-target reproduction and protocol validity before relaxing parser behavior; see `reproducer-validation-conventions.md`.
- !10523: strong corroboration of existing allocator-scope guidance because MySQL connection state must not retain packet-scope strings.

Additional strong corroboration came from Guy Harris's optional-libpcap and POSIX-shell fixes (!10557/!10558, !10552/!10555, !10533/!10535), named dissector registration in !10543, John Thacker's `phton32()` review in !10549, complete `wmem_tree` key shape in !10562, and test-infrastructure coordination in !10526.

Batch status: 44 merged and six closed/unmerged (!10559, !10542, !10541, !10531, !10525, !10522). Closed work was down-weighted. !10512 was inspected only as the next-frontier probe.
