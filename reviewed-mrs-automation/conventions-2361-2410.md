# Convention synthesis — Wireshark MRs !2361–!2410

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Wiretap / capture-file boundaries

- Keep `WTAP_ENCAP_*` values and pcap/pcapng LINKTYPE values in separate semantic domains. Translate at the file-format boundary only. Evidence: merged !2369, Guy-authored !2390/!2391.
- A pcapng packet's encapsulation must match the referenced interface description's encapsulation; reject mismatches as an internal caller error. Evidence: Guy-authored !2388 with stable counterparts !2392/!2393.
- Export semantic capability predicates such as "can this file type write this encapsulation" rather than exposing lower-level representation-conversion helpers to unrelated callers. Evidence: Guy-authored !2404.
- Track `err_info` allocation/free semantics alongside the numeric Wiretap error. Evidence: Guy-authored !2389/!2394.

## API / ABI boundaries

- Public ABI is intentional: built-in compatibility helpers are not plugin APIs merely because they live in a shared library. Evidence: Guy-authored !2387.
- Prefer caller-facing semantic facades and local ownership of operation state over making callers initialize internal helper structures. Evidence: Guy-authored !2401/!2402/!2404.

## Registered-field modeling

- For a one-bit masked field, prefer `FT_BOOLEAN` and specify the containing field width (8/16/32 as applicable) plus the mask; do not use integer/unit formatting to encode boolean semantics. Evidence: Pascal Quantin review and accepted revision in merged !2405.
- Prefer `VALS()`, standard bitmask/tree APIs, and field display metadata over bespoke post-add formatting/callback machinery when the standard APIs can express the protocol. Evidence: Alexis La Goutte, Pascal Quantin, and Anders Broman review in !2405.
- If code needs the decoded value immediately after adding a registered field, use the matching `proto_tree_add_item_ret_*` helper. Evidence: Anders Broman review in merged !2363.

## CLI and platform I/O

- Treat dynamic option enumeration as informational output rather than an error; provide an explicit discoverable query spelling such as `?` where appropriate. Evidence: Guy-authored !2381–!2383.
- On Windows, inspect native error state when libc's errno mapping is lossy. Preserve normal pipeline-close semantics and report other failures with the native message. Evidence: Guy-authored !2374–!2376.

## Tooling and submission

- Make local hooks thin adapters to canonical repository validators so local checks and CI enforce the same rule. Test the invocation path on Windows as well as Unix-like systems. Evidence: merged !2378 with Pascal Quantin portability feedback.
- For large feature MRs, squash work-in-progress history before merge when reviewers request a single coherent change, and prefer standard Wireshark APIs over custom local abstractions. Evidence: !2405.

## Weighting notes

- !2399 and !2366 were closed/unmerged and are retained only as lower-weight historical/negative evidence.
- Repeated stable backports are corroborating evidence, not separate independent architecture decisions.
