# Wireshark Generated-Output Conventions

This file records durable conventions for repository tools and generators that produce files. Current upstream tools remain authoritative.

## Construct fallible generated output before replacing the destination

A generator should not leave a syntactically valid-looking but incomplete output file merely because an exception or conversion error occurred halfway through generation. Perform fallible parsing, conversion, and output construction before committing the resulting bytes to the destination whenever practical.

Merged MR !22969, authored, approved, and merged by Gerald Combs, fixes `make-dmx-manfid.py` both for Python string handling and for partial-output behavior. The accepted change collects the generated output before writing it so an exception during generation cannot leave a partially rewritten file behind.

**Implementation rule:** separate generation from publication. Build and validate the complete output first; only after generation succeeds should the tool replace/write the destination. For larger outputs where buffering everything is inappropriate, use an equivalent transactional strategy such as writing a temporary file and atomically replacing the destination after success.

**Confidence:** Very high. Merged master tooling fix authored and merged by Gerald Combs with the partial-file failure mode stated explicitly.

## Validate generated output against cheap structural invariants before publication

A generator can complete successfully yet still produce catastrophically incomplete output when a parser or dependency regresses. When a stable generated artifact has an inexpensive structural invariant, check it before replacing the known-good output rather than assuming successful execution implies semantically plausible generation.

Merged master MR !21228, authored, approved, and merged by Gerald Combs, adds a guard to `make-enums.py` after PyClibrary 0.3.0 could produce fewer generated lines than the existing output. The generator compares the proposed line count to the existing file and refuses to overwrite it when the result unexpectedly shrinks. Stable-branch variants !21233-!21235 carry the same protection.

**Implementation rule:** for generated repository artifacts, add cheap sanity checks that detect implausible truncation or structural loss before publication. The invariant should reflect the artifact's expected evolution rather than being a generic requirement that output can never shrink; when legitimate shrinkage is possible, use a more semantic check or make the exceptional update explicit.

**Confidence:** Very high. Merged master safeguard authored and merged by Gerald Combs, followed by accepted stable-branch backports.