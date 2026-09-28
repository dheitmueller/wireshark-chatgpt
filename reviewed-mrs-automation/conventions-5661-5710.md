# Durable conventions from Wireshark MRs 5661-5710
Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Reserve ambiguous syntax rather than consuming it accidentally
Merged 5670 changes display-filter absolute-time output to ISO 8601 while retaining older input forms. John Thacker points out that separator-free ISO 8601 basic timestamps can be indistinguishable from plain numeric epoch seconds, a syntax Wireshark may want later. João Valverde agrees; merged 5689 requires separators for filter ISO 8601 timestamps.

Rule: when two plausible literal domains overlap lexically, prefer an unambiguous accepted form and leave the ambiguous spelling unused. When changing the preferred representation, preserve established parseable forms where practical.

## Test timezone arithmetic against independently known instants
Merged 5668, authored by John Thacker, fixes the sign of positive and negative timezone offsets. The existing tests had the same sign misconception and therefore validated the bug. Stable 5669 carries the correction to release-3.6.

Rule: for sign-sensitive time conversions, test several textual representations against independently known UTC instants, including positive, negative, fractional-hour, and zero offsets. Do not derive expected values with the same transformation logic under test.

## Keep library lifecycle ownership explicit until init/cleanup ordering is proven
Merged 5683 reverts an attempt to make epan initialize and clean up Wiretap because the change caused an exit crash. Explicit consumers still initialize Wiretap before libwireshark.

Rule: moving one library's init/cleanup into another library's lifecycle is an ownership change, not mechanical deduplication. Audit every executable, plugin/load path, repeated-init case, and teardown order before centralizing it.

## Static checkers must distinguish templates from final translation units
Merged 5663 exposed validate-clang-check.sh trying to validate ASN.1 packet templates as standalone generated dissectors. John Thacker identifies the mismatch, and Jaap Keuter explicitly recommends skipping all packet template C files rather than maintaining an ASTERIX-only exception.

Rule: source discovery belongs to the checker's contract. Match the actual compilation/semantic domain globally; do not accumulate protocol-specific exclusions for a whole class of generated templates.

## Prefer common user contracts and common implementation helpers
Merged 5704 uses the same -F output-format contract in text2pcap that sibling CLI tools already expose. Merged 5664 moves shared text-import SHB/IDB setup into the common text-import layer so CLI and GUI can reuse it, while documenting caller ownership of allocated IDB information.

Rule: when sibling frontends expose the same concept, prefer common option vocabulary and a common execution helper. A helper extraction is complete only when ownership and required call ordering are documented.

## Validate before resource acquisition when rejection can be decided first
Merged 5701 fixes a leak by moving a simple index-range check ahead of allocation.

Rule: put cheap structural or bounds validation before allocations or registrations whenever validation does not depend on the resource.

## UI styling should preserve semantic color meaning and platform/theme behavior
Merged 5700, authored by Gerald Combs, centralizes hover-color computation in ColorUtils. Merged 5699 removes decorative alternating rows because Wireshark already uses color to convey packet meaning. Both implement concerns raised during still-open 5685 review.

Rule: centralize theme/platform palette policy and avoid decorative color treatments that compete with established semantic coloring.

## Treat build-option renames as user-facing compatibility changes
Merged 5677 converts mixed DISABLE flags to consistent positive ENABLE semantics while preserving defaults. Alexis La Goutte requests release-note coverage and the final MR adds it.

Rule: build configuration names are an external interface used by CI, packagers, and developers. Keep polarity consistent, update repository consumers atomically, preserve defaults unless intentionally changing them, and document renames.

## Platform manifest features still require support-floor analysis
Merged 5678 enables the Windows UTF-8 active code page through the executable manifest. Guy Harris explicitly raises Microsoft's warning that older Windows builds still require legacy conversion handling; Gerald Combs identifies Wireshark's wide-character API paths through GLib and Qt.

Rule: a platform manifest setting is not a substitute for auditing behavior on every supported OS version. Review fallback behavior where the setting is ignored or partially supported.
