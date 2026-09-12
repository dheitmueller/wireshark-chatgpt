# Wireshark Generated-Output Conventions

This file records durable conventions for repository tools and generators that produce files. Current upstream tools remain authoritative.

## Construct fallible generated output before replacing the destination

A generator should not leave a syntactically valid-looking but incomplete output file merely because an exception or conversion error occurred halfway through generation. Perform fallible parsing, conversion, and output construction before committing the resulting bytes to the destination whenever practical.

Merged MR !22969, authored, approved, and merged by Gerald Combs, fixes `make-dmx-manfid.py` both for Python string handling and for partial-output behavior. The accepted change collects the generated output before writing it so an exception during generation cannot leave a partially rewritten file behind.

**Implementation rule:** separate generation from publication. Build and validate the complete output first; only after generation succeeds should the tool replace/write the destination. For larger outputs where buffering everything is inappropriate, use an equivalent transactional strategy such as writing a temporary file and atomically replacing the destination after success.

**Confidence:** Very high. Merged master tooling fix authored and merged by Gerald Combs with the partial-file failure mode stated explicitly.