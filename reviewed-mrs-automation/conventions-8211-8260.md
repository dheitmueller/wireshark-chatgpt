# Durable conventions from Wireshark MRs !8211-!8260

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Text encoding

Merged !8247 documents that Wireshark text is UTF-8 internally. Packet text should be converted or validated when it enters the text API, not repaired later by presentation formatting. Merged !8211 applies the same rule to SMB by converting UTF-16 and OEM byte strings through the standard string APIs. Merged !8217 shows that packet byte length and decoded UTF-8 length must be tracked separately because sanitization can change the representation length.

## Fuzz harnesses

Merged master !8249 bounds fuzzing of very large captures to a random packet subset. Follow-up !8256 preserves the intended optional command-line argument semantics, and !8259 resets range-selection state before each capture. Treat each input iteration as a fresh testcase configuration; state from one capture must not leak into the next.

## Dissector registration and submission

During merged !8242, review narrowed SAPNI's default port registration because the broader vendor port family is proprietary and not IANA-assigned. Prefer conservative defaults and user configuration or Decode As for deployment-specific unassigned ranges. The same review requested representative captures and kept those review pcaps attached to the merge request rather than committed to the source tree.

## Qt signal connections

Merged !8233 moved Capture-menu actions toward explicit typed Qt connections, but post-merge testing found old string-based callers of a removed slot elsewhere in the tree. When deleting or renaming a slot, search all call sites, including legacy string connections and parallel frontends. Typed connections only give compile-time checking at sites that have actually been migrated.

## Boolean fields

In merged !8212, John Thacker noted that a field converted to FT_BOOLEAN should use a true_false_string rather than an integer value table. First establish that the protocol field is genuinely Boolean; once it is, use the Boolean-specific display metadata.

## Integer domains

Merged master !8248 fixes Opus calculations because 48000 does not fit in a signed 16-bit parameter. Choose C integer width and signedness from the actual semantic range, not from a superficially similar neighboring field.

## Coordinate hygiene

Merged !8218 fixes an HTTP regression where an offset was used as a string length. Offsets, wire lengths, and decoded-text lengths are separate semantic quantities even when they share the same C integer representation.

## Source ranges

Merged !8236 corrects a ROHC protocol-tree item from four highlighted bits to the three bits actually consumed. Tree source ranges should represent the exact packet bits or bytes that determine the displayed value.
