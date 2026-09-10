# Wireshark Privilege Boundary Conventions

This file records durable privilege-boundary and attack-surface conventions extracted from accepted upstream Wireshark changes. Current upstream architecture remains authoritative.

## Keep convenience functionality out of elevated capture components when it can be composed externally

Features implemented inside a privileged helper carry a larger security cost than equivalent features implemented in an ordinary process. When functionality is not fundamental to the privileged component's job and can be reproduced by unprivileged standard tools feeding an existing interface, removing it can be preferable to maintaining another parser, protocol path, or network-facing capability inside the privilege boundary.

Merged MR !24767 removes dumpcap's `TCP@` interface specifically to reduce dumpcap's attack surface, noting that dumpcap commonly runs with elevated privileges on Linux. The same workflow can be reproduced with tools such as `nc`, `ncat`, `socat`, or `/dev/tcp` piped to standard input, so the convenience feature did not justify retaining an extra network-facing path in the elevated process.

**Architecture rule:** apply least functionality as well as least privilege to capture helpers. Before adding or retaining convenience behavior in dumpcap or another elevated component, ask whether it is essential to the privileged operation; if an unprivileged process can perform it and feed an existing narrow interface, prefer that composition.

**Confidence:** Very high. Explicit security rationale in a merged master change, accepted by Wireshark maintainers.
