# Wireshark Command-Line Exit-Status Conventions

This file records durable command-line status and control-flow conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Keep process exit statuses semantically specific, and keep internal control sentinels outside the exit-status domain

Command-line tools should return status values that identify the actual failure class rather than reusing a historical or convenient numeric value with a different meaning. Internal control-flow signals are a separate domain and should not accidentally be valid process statuses.

Merged master MR !23639, authored and merged by Guy Harris, expands tshark's `WS_EXIT_*` statuses so errors such as invalid filters do not return status 2 merely because that number already existed for an invalid capture interface. The same change moves `WS_EXIT_NOW` outside the valid UNIX exit-status range because it is not itself an exit status: it means option processing has already printed the requested information successfully and the caller should terminate with `EXIT_SUCCESS`. Earlier merged !23631 and its supported-branch backports !23632/!23633 establish the same semantic-status direction for invalid read filters.

**Implementation rule:** use a named process-exit status whose meaning matches the condition being reported. Do not overload an existing status for an unrelated failure just because its numeric value is available.

**Control-flow rule:** if an API also needs a sentinel such as “output completed; return success now”, model that sentinel so it cannot be mistaken for a real process exit code. Callers should translate the sentinel to the appropriate external result rather than pass it through verbatim.

**Review rule:** when adding exit codes, audit callers and comments for assumptions that every enum/member is externally observable as an OS exit status; internal control values and public exit codes should remain visibly distinct semantic domains.

**Confidence:** Extremely high. The principal merged master change was authored and merged by Guy Harris, and the direction is independently corroborated by the merged invalid-filter exit-status fix family.

## Treat output failure as a terminal processing result, not merely a stream flag checked later

For command-line packet-processing tools, successful dissection and successful delivery of its output are different results. If `print_packet()` or the corresponding flush fails, propagate a distinct output-error state immediately and stop processing additional packets. Continuing to dissect after a broken pipe, full filesystem, or exceeded quota wastes work and can obscure the real failure.

Merged master MR !23569, authored and merged by Guy Harris, replaces a boolean packet-processing result with an enum that distinguishes “passed filter”, “did not pass”, and “passed but printing failed”. It explicitly checks both packet printing and `fflush()` and terminates packet processing on the print-error result. Guy's accepted release-4.6 backport !23570 carries the same behavior.

**Implementation rule:** make output-producing processing APIs capable of reporting output failure as a first-class result. Check the operation that can fail at the point it occurs, propagate that result through the processing loop, and stop work when the consumer or output medium can no longer accept data.

**Confidence:** Extremely high. Merged master and supported-branch changes authored and merged by Guy Harris with explicit rationale for broken-pipe, ENOSPC, and quota failures.
