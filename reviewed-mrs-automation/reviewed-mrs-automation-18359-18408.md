# Automated MR review: !18359–!18408

- Corpus repository: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`
- Direction: newest toward older
- Selection method: rebuilt the already-reviewed set from `reviewed-mrs-automation/` and `reviewed-mrs.md`; preserved/counts the historical !17571–!17620 batch. Selected the 50 highest-numbered corpus MRs not in that set.
- Exact reviewed set (50): !18408, !18407, !18406, !18405, !18404, !18403, !18402, !18401, !18400, !18399, !18398, !18397, !18396, !18395, !18394, !18393, !18392, !18391, !18390, !18389, !18388, !18387, !18386, !18385, !18384, !18383, !18382, !18381, !18380, !18379, !18378, !18377, !18376, !18375, !18374, !18373, !18372, !18371, !18370, !18369, !18368, !18367, !18366, !18365, !18364, !18363, !18362, !18361, !18360, !18359.

## Review notes

Merged MRs were weighted above closed, abandoned, or superseded work. No Guy Harris-authored/reviewed item in this batch produced a stronger new durable convention than rules already present in the notebook.

### !18408 — TLS: add SecP384r1MLKEM1024

Merged. A deliberately small registry update adding the draft TLS named group to the existing value table. Useful as an example of keeping protocol registry additions narrow and source-identified, but no new notebook rule is needed.

### !18395 — FlexRay: avoid allocating keys

Closed/unmerged. Proposed replacing allocated 64-bit hash keys with GLib direct-pointer keys. The author closed it with the explicit note that the changes would instead be done in a single MR. Treat this as superseded workflow context, not an implementation exemplar.

### !18380 — MBIM: stop NDP parsing at first Null entry

Merged release-4.4 backport. The protocol specification says the first Null datagram-pointer entry terminates the meaningful list; implementations may leave later fixed-size slots uninitialized. The accepted parser stops at that semantic terminator and exposes the remaining bytes as padding rather than interpreting junk and producing false diagnostics. This strongly corroborates existing defensive parsing guidance: protocol-defined terminators override apparently parseable trailing bytes, and uninterpreted framing/padding should not be promoted into semantic fields.

### !18370 — Stratoshark Qt MOC preference types

Merged, John Thacker-authored. Applies the corresponding Wireshark fix to Stratoshark by using the typedef names (`pref_t`, `module_t`) expected by Qt MOC-generated signal/slot code. Useful reminder to audit sibling front ends when shared UI/API type changes affect generated bindings, but too implementation-specific for a new durable convention.

### !18359 — PTP management TLV offset handling

Merged. Refactors management-TLV parsing away from a set of absolute offset macros toward consistent local offset progression. This reduces duplicated offset arithmetic and makes parsing state correspond to bytes actually consumed. It corroborates existing parser guidance favoring local, monotonic consumption state over scattered absolute-offset constants.

## Notebook impact

No convention file changed in this run. The strongest findings corroborate existing defensive-parsing, state/offset, and cross-front-end consistency guidance rather than establishing a genuinely new rule.

## Continuation

On the next run, rebuild the reviewed set again from all ledgers rather than assuming this numeric range or any other range is complete. Continue with the highest-numbered corpus MRs not present in that rebuilt set; absent intervening gaps/new tracking, the next candidate is !18358.
