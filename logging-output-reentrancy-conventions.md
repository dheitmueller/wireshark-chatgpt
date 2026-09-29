# Wireshark Logging Output and Reentrancy Conventions

Merged master MR !5435, authored by João Valverde, makes stderr the default diagnostic stream so command-line stdout remains safe for normal output and pipelines. Explicit stdout exceptions remain only where compatibility or protocol behavior requires them, such as extcap and the GUI compatibility path.

Merged master MR !5448 adds a second constraint: helpers used while emitting a log message must not call code that can itself log. Its timestamp helper therefore uses low-level platform time APIs and fallbacks rather than logging-capable dependencies.

**Rules:** default CLI diagnostics to stderr unless stdout is an explicit contract. Audit log-writer and log-handler helpers for indirect re-entry into logging, and use non-logging primitives on that path.

**Confidence:** Very high; both are merged master logging changes with explicit rationale.
