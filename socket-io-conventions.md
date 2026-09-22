# Wireshark Socket I/O Conventions

This file records durable conventions for portable socket I/O extracted from accepted upstream Wireshark changes. Current platform APIs and upstream source remain authoritative.

## Treat nonblocking connect readiness as completion, then query the actual socket error

A nonblocking `connect()` does not turn writability into a success indication. On POSIX systems, after `connect()` returns `EINPROGRESS`, `select()`/`poll()` readiness means that the asynchronous connection attempt has completed; the completion may be either successful or failed. Code must retrieve `SO_ERROR` with `getsockopt(SOL_SOCKET, SO_ERROR, ...)` to distinguish those outcomes.

Merged master MR !13697, authored and merged by John Thacker, changes `androiddump` from long blocking loopback connects on UNIX-like systems to nonblocking connects. Guy Harris gave especially authoritative review by pointing to the POSIX asynchronous-connect contract. John then noted the important portability distinction: UNIX readiness can report writability after either success or failure, whereas the existing Windows `select()` path used different `writefds`/`exceptfds` behavior. The accepted implementation explicitly performs the proper UNIX completion check rather than copying the Windows readiness interpretation.

**Implementation rule:** when completing a nonblocking POSIX `connect()`, wait for the platform-defined readiness condition and then inspect `SO_ERROR`; zero means success and a nonzero socket error is the connection failure. Do not equate a writable socket with a successful connect.

**Portability rule:** model the semantic operation—"asynchronous connect completed; obtain its result"—and implement it with the platform's actual readiness/error contract. Do not assume that Windows and POSIX `select()` classify failed connection attempts into identical fd sets.

**Testing rule:** exercise success, immediate refusal, timeout/unreachable, and loopback/interface-down cases. A timeout optimization is not correct if a failed asynchronous completion is accidentally treated as a successful connection.

**Confidence:** Extremely high. Merged master fix authored and merged by John Thacker with direct POSIX-contract review from Guy Harris and an explicit accepted correction for the UNIX completion semantics.