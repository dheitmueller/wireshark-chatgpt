# Wireshark Dependency Version-Parsing Conventions

## Compare dotted numeric versions component-wise

Merged master MR !6445, authored by Stig Bjørlykke, fixes Gcrypt feature detection that converted a major.minor version string to floating point. A version such as 1.10 then compares as 1.1, which is not version ordering. The accepted code parses major and minor separately and compares integer tuples.

Closed MR !6441 first proposed a general Python version library. Gerald Combs found that introducing that dependency broke Windows test startup and suggested the tuple representation because this particular input is known to be an integer pair. Stig then adopted the simpler design in !6445.

**Rule:** never use floating-point conversion for dotted version ordering. Parse version components and compare them structurally, or use an approved version library when the actual grammar requires richer semantics.

**Dependency rule:** do not add a cross-platform dependency solely to solve a bounded problem that has a small explicit representation.

**Confidence:** Very high for merged !6445; !6441 is supporting and superseded review evidence only.
