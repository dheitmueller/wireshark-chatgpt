# Wireshark review findings: !10812 through !10763

This file records durable conventions extracted from this review batch. Merged master changes are treated as the primary evidence.

## Variable-length integer parsing

Merged !10808, authored and merged by John Thacker, reinforces two related rules. Arithmetic must have the required width at the operation itself rather than only in the destination variable. A variable-length integer parser also needs an independent limit on the number of encoded components, because a continuation sequence can leave the accumulated value unchanged while the component count continues to grow.

Review variable-length integer decoders for both value range and encoded-length range. Test degenerate continuation sequences in addition to maximum ordinary values.

## Request-selected response formats

Merged !10806, authored and merged by John Thacker, shows that response grammar should be carried as explicit protocol state when a preceding request selects it. In MySQL, text and binary result rows can share the same initial byte pattern, so the response alone is not always sufficient to identify the format.

Record the selected mode on the first pass and preserve the information needed by each response frame so later random-access or second-pass dissection gives the same result without depending on visitation order.

## Converted text lengths

Merged !10784, authored and merged by John Thacker, reinforces that the byte length of encoded packet text and the byte length of its converted representation are separate quantities. Once text has been converted, downstream storage and copying must use the length of the converted representation rather than assuming the original wire length is still correct.

## Formatting without an avoidable intermediate

Merged !10782, authored and merged by John Thacker, removes a fixed-size intermediate formatting step before appending text to protocol items. When the destination API can accept the original formatting inputs directly, an additional intermediate buffer creates an unnecessary truncation boundary and can damage multibyte text.

Prefer direct formatting into the destination API when that API already owns the formatting and text-handling contract. Avoid formatting a string through an extra formatting layer when the operation is simply to add the existing string unchanged.

## Generated-source parity

Merged !10780, authored by John Thacker and merged by Gerald Combs, is an early direct precedent for Wireshark's generated-source CI policy. The Code Checks job regenerates ASN.1 dissectors from their authoritative inputs and fails if tracked output changes. This corroborates the broader generated-code convention already recorded elsewhere in the notebook.

## Submission and review corroboration

Merged !10765, !10797, and !10800 reinforce the expectation that protocol changes should have representative capture material where practical. The detailed !10765 review also shows attention to project assertions, consistent field naming, value mappings, and readable field descriptions.

The still-open draft !10792 is lower-weight evidence. John Thacker explicitly narrowed that broader proposal before the accepted HTTP/2 Export Objects change in merged !10802, providing a useful example of reducing MR scope before merge.
