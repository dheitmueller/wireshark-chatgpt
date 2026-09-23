# Wireshark File Identity Safety Conventions

This file records durable conventions for tools that read and write capture files or other user-selected filesystem objects. Current upstream source remains authoritative.

## Compare filesystem identity, not path spelling, before self-feeding write operations

A tool that reads one or more files while appending to an output must reject cases where the output is one of the inputs. Comparing pathname strings is not sufficient for that safety check: distinct path spellings can still designate the same underlying file. Use the project's file-identity helper at the boundary where the destructive or self-feeding operation would begin.

Merged master MR !12769 fixes `mergecap` after append mode could be pointed at an input file and then continually consume the data it was appending. The accepted implementation checks the output against every input with `files_identical()` before entering the merge loop and returns `MERGE_ERR_INVALID_OPTION` with a user-facing diagnostic when identity matches. The final change was committed and approved by Guy Harris, giving the behavior unusually strong review authority.

**Implementation rule:** before an operation can overwrite, append to, or otherwise mutate a file while simultaneously consuming named inputs, compare actual file identity for every input/output combination whose aliasing would be unsafe. Prefer Wireshark's shared identity helper over raw string equality so the safety invariant follows filesystem identity rather than path syntax.

**Review rule:** treat input/output aliasing as an API precondition, not as an obscure runtime failure mode. Reject the operation before opening the feedback-producing write path, and return a specific error that explains why the requested combination is invalid.

**Confidence:** Extremely high. Merged master correctness fix with a concrete infinite-loop failure mode, committed and approved by Guy Harris.
