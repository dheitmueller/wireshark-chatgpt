# Wireshark Value-String Domain Conventions

This file records durable conventions for keeping symbolic value mappings consistent with registered field domains. Current upstream source remains authoritative.

## Value-string entries must fit the field's logical domain

Merged master MR !9837, authored by Martin Mathieson, fixes checker findings where `value_string` entries were not representable by their registered fields. The accepted changes widen fields whose semantic values genuinely require more bits, correct erroneous table values and ranges, and leave an unusual GMR-1 case explicitly documented rather than applying a mechanical conversion. Guy Harris's merged release backports !9858 and !9860 carry the same corrections, while Guy's merged !9845 documents the specification ambiguity behind the GMR-1 0x100 convention.

**Implementation rule:** review the registered field type, mask, value actually added to the tree, and `VALS` table as one contract. If a table entry falls outside the effective logical field domain, determine whether the field is too narrow, the mapping is wrong, or the protocol has an intentional exceptional encoding.

**Review rule:** checker findings require semantic triage. Do not automatically widen a field when the real problem may be the table or protocol interpretation.

**Confidence:** Extremely high. Merged master cleanup with accepted stable backports and a Guy Harris-authored explanation of the exceptional case.
