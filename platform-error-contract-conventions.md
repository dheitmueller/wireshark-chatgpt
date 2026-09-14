# Platform Error-Contract Conventions

This note records durable guidance for platform-specific system API error handling in Wireshark. Current upstream source and platform API documentation remain authoritative.

## Read failure status from the platform API's documented error channel

Do not assume POSIX error-reporting conventions apply to a platform compatibility API merely because the operation has a familiar socket/file shape. Test the documented failure sentinel first and retrieve the corresponding error code through the API-defined mechanism.

Merged master MR !20250, authored and merged by John Thacker, fixes `androiddump` on Windows. Winsock errors are not reported reliably through the C `errno` variable; `recv()` must be checked for `SOCKET_ERROR` and the thread-local socket error obtained using `WSAGetLastError()`. The earlier code could therefore misclassify timeout/error conditions on Windows.

Merged follow-up !20251 safely restores timeout handling for non-blocking Windows sockets once the error source is interpreted correctly. Release-4.4 backport !20263 carries the same `WSAGetLastError()` correction into the stable branch.

**Implementation rule:** preserve the native error contract across platform branches. For Winsock, use Winsock return values and Winsock error retrieval; for POSIX APIs, use their documented `errno` behavior. Normalize into a common project error domain only after the platform-specific failure has been captured correctly.

**Review rule:** when portable code has `#ifdef` branches around system calls, audit not only argument/types but also return sentinels, error storage, timeout/nonblocking codes, and the point at which volatile error state is captured.

**Confidence:** Very high. Merged master fix and follow-up authored by John Thacker plus an accepted stable backport, with the Windows API contract explicitly cited in the MR rationale.
