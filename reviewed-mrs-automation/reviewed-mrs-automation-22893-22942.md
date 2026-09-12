# Automated Wireshark MR review: !22893-!22942

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `9e52bc78659a888d4eb624984ee1a886a40d959f`

Review direction: newest to older.

Exactly 50 previously unreviewed MRs were selected by rebuilding the already-reviewed set from the available review tracking and then taking the highest-numbered corpus MRs not already present. The historical !17571-!17620 batch remains part of the reviewed set.

## Exact reviewed set

!22942, !22941, !22940, !22939, !22938, !22937, !22936, !22935, !22934, !22933, !22932, !22931, !22930, !22929, !22928, !22927, !22926, !22925, !22924, !22923, !22922, !22921, !22920, !22919, !22918, !22917, !22916, !22915, !22914, !22913, !22912, !22911, !22910, !22909, !22908, !22907, !22906, !22905, !22904, !22903, !22902, !22901, !22900, !22899, !22898, !22897, !22896, !22895, !22894, !22893

## Durable findings promoted

### !22933 — validate public headers independently

Merged master MR !22933 adds an executable check that `wsutil` public headers compile without relying on libwiretap/libwireshark headers being included first. Guy Harris explicitly argued during review that this kind of public-header validation should ideally run on all build platforms rather than exist only as a Debian packaging check. Promoted to `public-header-validation-conventions.md`.

### !22894 — protocol-tree source ranges must describe real encoded bytes

Merged master MR !22894 fixes CBOR bitmask parent/child items whose offsets and lengths were tied to a virtual child TVBuff. The accepted implementation anchors them to the actual CBOR integer bytes, fixing `-T jsonraw` source ranges without changing decoding or display-filter semantics. Promoted to `protocol-item-source-range-conventions.md`.

## Strong corroborating findings

- !22932, !22930, !22924, !22916, !22914, !22900, and !22898 are part of the unsigned-TVBuff API migration and reinforce existing unsigned-domain, remaining-length, and overflow-safe bounds guidance. !22924 is especially explicit that an overlarge maximum search length should be trimmed to captured bytes rather than allowed to overflow.
- !22896 reinforces the existing preference for bounded subset TVBuffs: passing a descriptor-bounded subset naturally constrains nested searches and removes duplicated end-offset checks.
- !22929, !22926, and !22921 reinforce existing header/interface rules. Public headers cannot rely on private `config.h` feature macros, and headers should directly include authoritative declarations for public types they expose.
- !22902 checks all `file_seek()` failures and marks the fallible function `WS_WARN_UNUSED`, reinforcing the practice of making required error handling compiler-enforceable where practical.
- !22903 fixes ownership cleanup on an early-return failure path; !22931 fixes allocator-sensitive leaks. Both corroborate existing lifetime/cleanup guidance.
- !22915 checks libgcrypt error returns rather than assuming cryptographic operations succeed.
- !22905 is deliberately treated as a static-analysis false-positive suppression rather than evidence for changing correct GLib semantics: `g_strjoin()` legitimately uses NULL as its variadic terminator.
- !22917 shows that a newly introduced failure sentinel must be compatible with all callers; returning `-1` was wrong when callers treated the helper as an end-offset producer.
- !22912 prevents emitting a bounded textual field ending in a partial UTF-8 code point, corroborating existing text-boundary guidance.
- !22907, !22906, and !22899 fix display-filter scalar conversion crashes by producing type-conversion errors instead of reaching invalid ftype assertions, corroborating parser/type-system validation guidance.
- !22920 fixes BGP add-path detection by using the parser's current cursor when computing remaining bytes, reinforcing correct parser-coordinate accounting.
- !22893 avoids post-decrement loop tests that permit the tested unsigned variable to wrap on loop exit, corroborating arithmetic-safety guidance.

## Weighting / non-promotion notes

- !22897 was a draft and closed unmerged. Its unsigned ptvcursor direction was down-weighted and not treated as accepted architecture.
- Straightforward protocol additions, generated-value updates, UI/resource updates, backports, dead-store fixes, and narrowly local Coverity repairs in the remainder of the batch were reviewed and recorded but did not justify new notebook rules where stronger existing guidance already covered the lesson.
- Merged MRs and direct maintainer rationale were weighted more heavily than closed/draft work. Guy Harris's substantive comments on !22933 were given especially high authority.
