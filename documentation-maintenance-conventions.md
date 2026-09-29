# Wireshark Documentation Maintenance Conventions

This file records durable review conventions for documentation structure and mechanically maintained source comments.

## Let documentation tooling derive file identity

Merged master MR !5217 adds Doxygen `@file` markers to exported headers. During review, Jaap Keuter points out that filenames embedded in source comments are frequently forgotten or become wrong after files are moved or renamed, while Doxygen does not require the filename after `@file`. Gerald Combs explicitly favors bare `@file` so Doxygen derives the filename itself, and the accepted revision removes the duplicated names.

**Documentation rule:** do not manually repeat metadata that the documentation tool can reliably derive, especially filenames tied to source paths. Prefer bare structural markers when that avoids a second copy that can drift.

## Agree on maintainable policy before sweeping mechanical cleanup

In the same !5217 review, Jaap Keuter cautions that a broad documentation rewrite should follow a documentation policy the project agrees on and is willing to maintain. The accepted change converged on a simple convention rather than preserving inconsistent filename annotations.

**Review rule:** before a repository-wide mechanical documentation change, establish the intended long-term convention and make the transformation serve that policy. Consistency alone is not enough if the chosen form creates recurring maintenance burden.

**Confidence:** Very high. Direct Jaap Keuter and Gerald Combs review on a merged master documentation change.
