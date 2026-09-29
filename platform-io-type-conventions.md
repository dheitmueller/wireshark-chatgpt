# Wireshark Platform I/O Type Conventions

## Match wrapper integer types to the platform API's actual ABI

Merged MR !5514 adds POSIX compatibility support for ssize_t. During review Guy Harris points out that Windows' POSIX-like _read(), _write(), recv(), recvfrom(), send(), sendto(), and related functions return int, even though a generic ssize_t compatibility type may reasonably be wider on LLP64.

Merged MR !5520 follows by introducing ws_file_size_t and ws_file_ssize_t: they map to unsigned int / signed int for the Windows file APIs and size_t / ssize_t on POSIX.

**Implementation rule:** distinguish a generic compatibility type from the exact type of a platform API boundary. Project wrappers should match the real ABI on each platform; convert to wider internal accounting types explicitly after the call where needed.

**Review rule:** a warning fixed by changing an integer typedef should trigger a check of the underlying system-call signature on every supported platform. Do not assume POSIX names imply POSIX-width return types on Windows.

**Confidence:** Extremely high. Direct Guy Harris portability review, followed by a merged project wrapper design that encodes the distinction.
