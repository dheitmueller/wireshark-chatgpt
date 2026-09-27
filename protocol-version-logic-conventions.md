# Wireshark Protocol Version Logic Conventions

In merged MR !7885, John Thacker identified duplicated TLS version checks as the reason CCM AAD-length handling diverged between cipher setup and later authentication. The accepted implementation computes `aad_len` once, after the version/mode decisions, and reuses that value when configuring the cipher.

**Rule:** when a protocol parameter depends on version, draft revision, mode, or negotiated capability, derive the parameter once and pass or reuse the resulting value. Do not reproduce the same version predicate in multiple processing stages.

**Review implication:** when a bug fix adds a new version case, search for duplicated decision trees that compute the same semantic parameter elsewhere.

**Confidence:** Very high. Merged change with explicit John Thacker review identifying duplicated logic as the source of the miss.
