# Wireshark conventions from MRs 1360-1409

Current upstream code and tooling remain authoritative.

## Detect capabilities, not platform names

Guy Harris-authored !1404 and backport !1407 establish that API availability belongs to a concrete OS, C library, compiler, and SDK combination rather than an abstract operating-system family. Probe the API that will actually be called and keep a fallback for supported environments that lack it.

Guy Harris-authored !1392 and !1367 reinforce the same rule for optional library features: satisfying a version floor is not proof that a feature was compiled into the dependency. Check the actual capability when build options can remove it.

## Stage noisy checkers without hiding their output

Merged !1400 adds commit-scoped typed-item and true/false-string checks to merge-request CI while preserving reports as artifacts. Martin Mathieson kept them advisory at that stage because known pre-existing findings made immediate hard gating impractical.
