# Review findings: !4011–!4060

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

The exact 50-MR set is recorded in `reviewed-mrs-automation-4011-4060.md`. Durable findings extracted from this batch are recorded in `conventions-4011-4060.md`.

## High-value findings

- !4059 (merged): John Thacker required DVB-S2 heuristic ownership checks to be distinct from explicit Decode As behavior.
- !4057 (merged): John Thacker fixed an assertion by creating a composite TVB only when at least one component exists.
- !4052 (merged, Guy Harris): pcapng padded/serialized option-size semantics belong in the pcapng writer, not a generic Wiretap option API.
- !4050 (merged): command-line preference overrides survive Lua reload until a newer UI edit intentionally supersedes them.
- !4048 (merged): Anders Broman exposed the Windows path failure; Gerald Combs recommended `$<TARGET_FILE:lemon>` for the build-host Lemon generator.
- !4042 (merged, Guy Harris): every successful Wiretap record receives a block in the common read path; failure cleanup is centralized there too.
- !4036 (closed): Guy Harris rejected masking a race with NULL guards; Roland Knall found the real cause in excessive UAT model notifications, fixed later by merged !4135.
- !4024 (merged, Guy Harris): opposite-endian parsing must populate the same semantic record state as native-endian parsing.
- !4019 (merged, Guy Harris): serialize the semantic pcapng string value, not the wrapper structure that stores it.
- !4013/!4012 (merged stable backports): restore saved `pinfo->can_desegment` before nested AMQP dispatch so TCP PDU reassembly remains available.
- !4011 (merged): consolidate build guidance on canonical documentation/setup scripts; Guy Harris confirmed libpcap is optional from the actual CMake contract.

The remaining reviewed MRs were scanned for implementation and review evidence; routine backports, automatic updates, spelling/version changes, and changes already covered by stronger notebook rules did not justify additional durable conventions.
