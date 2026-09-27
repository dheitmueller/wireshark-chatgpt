# Conventions extracted from Wireshark !7861-!7910

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

## Endpoint tables, conversations, and identifiers are different abstractions

Merged !7874 and !7869, authored by Guy Harris, rename the old “host/hostlist” endpoint-table API to use endpoint terminology. An endpoint can be an address or an address/port pair; it is not necessarily a host. The review discussion also distinguishes an endpoint from a conversation identifier or virtual-circuit/connection identifier. A connection ID may identify a conversation without itself identifying either endpoint.

When public API terminology is corrected, preserve compatibility intentionally. These MRs keep deprecated wrapper functions and typedefs for old source/binary users and update Debian symbol metadata for newly exported names. Merged !7882 fixes an accidentally omitted compatibility declaration, while !7891/!7896 correct deprecation annotations.

**Rule:** name APIs after the semantic object they actually represent. Do not blur endpoint identity, conversation identity, and circuit/connection identifiers. For public renames, provide deliberate compatibility shims and keep export/symbol metadata synchronized.

## Advance reassembly frontiers across all newly contiguous data

Merged !7897, authored by John Thacker, fixes TCP out-of-order multi-segment-PDU tracking. When an arriving fragment fills a gap inside an in-progress PDU, later fragments that were already buffered may immediately become contiguous too.

**Rule:** a “highest contiguous sequence” value is a property of the stored fragment set, not just the newest fragment. After filling a gap, advance through all now-contiguous fragments before recording the new frontier.

## Persist state at PDU granularity when a frame can cross states more than once

Merged !7861, authored by John Thacker, fixes SMTP DATA/BDAT pipelining. One frame can contain several commands/data regions and switch parser state multiple times, so one stored PDU type per frame is insufficient. The accepted design stores an ordered list of PDU records with end offsets and parses the remaining semantic units on the first pass.

**Rule:** if multiple PDUs in one frame can begin under different protocol states, snapshot state per PDU and retain a stable boundary such as an offset. Frame-level state alone is not precise enough for later redissection.

## Compute version-dependent protocol parameters once

In merged !7885, John Thacker identifies duplicated TLS version checks as the reason CCM AAD-length handling diverged between cipher setup and authentication. The accepted code computes `aad_len` in one place and then reuses the value.

**Rule:** when a protocol parameter depends on version/draft/mode, derive it once and pass or reuse that value. Duplicating the same version predicate in multiple stages invites silent drift when one branch is updated without the other.

## Name portability limits for the constraint they actually express

Merged !7902 fixes 32-bit Linux timestamp handling. Guy Harris points out that the original `WTAP_NSTIME_SECS_MAX` name implied a universal timestamp maximum, while the code actually needed the maximum positive seconds value representable by a 32-bit field given the platform's `time_t` width. The accepted name becomes `WTAP_NSTIME_32BIT_SECS_MAX`.

**Rule:** name integer-limit constants after the actual receiving representation or protocol constraint, not an overbroad type concept. Derive the bound from the width/range that must safely receive the value.

## Heuristic recognition should validate structure before claiming ambiguous data

Merged !7907 tightens HTTP recognition when no request/response/header line has yet been established. A line ending plus colon is not sufficient; the candidate header name is validated before the dissector treats the data as HTTP.

**Rule:** for weak or mid-stream recognition paths, require structural validity appropriate to the candidate field before claiming a protocol. Avoid heuristics that turn generic punctuation in continuation data into false-positive protocol recognition.

## Keep diagnostic exceptions local to the dependency that needs them

Merged !7878 and its branch counterpart !7904 wrap a GCC 12.1 Qt6 false-positive warning suppression around the specific Qt include and restore the diagnostic immediately afterward.

This is earlier corroboration of the notebook's later warning-policy guidance: suppress or demote only the diagnostic that cannot reasonably be fixed in project code, and use the narrowest source scope possible.

## Tooling gates depend on the debug-information/toolchain contract

Merged !7880/!7881 and !7879/!7887 pin DWARF-4 where ABI Dumper 1.2 or the selected Valgrind could not consume the compiler's DWARF-5 output. !7892 temporarily disables a demonstrably broken ABI gate while leaving its definition in place; !7893 investigates the compiler/debug-info/tool-version interaction.

These MRs are corroboration for later, stronger ABI-tooling guidance: analyzer output is only meaningful when the compiler's emitted format is supported by the analysis tool, and CI migrations must validate that contract explicitly.

## Closed CMake rewrites are negative evidence, not exemplars

Closed !7866 proposed replacing parts of GnuTLS discovery. João Valverde objected that it removed pkg-config-derived variables, misinterpreted Windows-specific `GNUTLS_HINTS`, and added `PATH_SUFFIXES` without demonstrating the real failure. The MR was closed.

**Review rule:** for dependency-discovery changes, require a concrete failing configuration and understand existing platform/package-manager semantics before replacing mechanisms. Do not treat a closed implementation as authoritative merely because its proposed cleanup looks simpler.
