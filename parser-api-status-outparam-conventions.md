# Parser API Status and Out-Parameter Conventions

This file records durable conventions for parser APIs that separate operation status from decoded values returned through output parameters.

## Keep status returns separate from value outputs

When an API is changed from returning a length/value directly to returning success/status while placing the former value in an output parameter, every caller must be audited for the semantic change. A boolean status is not a numeric length, and assigning it to the old length variable silently converts useful values into 0/1.

Merged MR !23768 repairs MGCP and MSN Messenger callers after `tvb_find_line_end_remaining()` changed to return `bool` and return the line length through an output argument. Tamas Regos explicitly pointed out in review that callers using the out-parameters should still handle the function's status-return contract; where the status is deliberately irrelevant, the accepted form explicitly discards it with `(void)` rather than pretending it is the length.

**Implementation rule:** during API migrations, map each old return value and each new output parameter by meaning, not merely by compatible C type. Use the output parameter for the decoded length/value and test the returned status when it affects control flow.

**Review rule:** if a status return is intentionally ignored because the output parameters and surrounding bounds checks fully define the caller's behavior, make that decision explicit (for example with a `(void)` cast) rather than assigning the status into a value-domain variable. This both documents intent and keeps static analysis useful.

This complements the notebook's existing rules that enum/status values are not booleans and that tri-state integer contracts must retain all states.

**Confidence:** High. Merged master correctness fix with a concrete regression caused by the API semantic change and a resolved review discussion specifying the accepted call pattern.

## Populate requested output parameters on every successful return path

An optional output parameter is part of the function's success contract whenever the caller supplies it. A parser fast path must not return successfully before filling an output that slower paths populate; otherwise behavior depends on how many bytes were needed to encode the same semantic value.

Merged master MR !13198, authored and merged by John Thacker, fixes QPACK prefixed-integer parsing where the Huffman-encoding flag was only written after the one-byte fast path. When the integer fit entirely in the prefix byte, the function returned before setting the requested flag. The accepted change moves the `out_flag` assignment ahead of that early return so all successful paths initialize it consistently.

**Implementation rule:** for functions with optional output pointers, identify which outputs are promised on success and initialize or populate them before any successful early return that would otherwise bypass the assignment. Do not let a compact encoding or fast path silently change the output-parameter contract.

**Review rule:** when a parser has early returns for short/common encodings, audit every requested output parameter across those paths, not only the primary decoded value and consumed length.

**Confidence:** Very high. Merged master correctness fix authored and merged by John Thacker; the bug directly depended on a successful fast path leaving caller-visible state unset.
