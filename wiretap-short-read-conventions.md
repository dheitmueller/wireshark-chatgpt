# Wireshark Wiretap Short-Read Conventions

This file records durable conventions for capture readers whose preferred transfer unit can be only partially filled by the final physical read.

## Distinguish a valid short final read from EOF or an I/O failure

A fixed-size page or block is often a preferred transfer unit rather than a guarantee that every physical read fills it. If the format permits a shorter final page, a positive short read is valid data and parsing must use the actual byte count; zero bytes indicate EOF; a negative result indicates an I/O failure.

Merged master MR !10901, authored by Guy Harris, fixes the NetScaler trace reader accordingly. The accepted reader requests the normal page size but accepts any positive result, records that returned length, treats zero as EOF, and reports negative reads through the file-error path. Its initial format probe also scans only the bytes actually returned rather than rejecting a short first page.

**Implementation rule:** keep requested buffer capacity separate from actual bytes read. Bound parsing and signature scanning by the actual returned length, and decide whether a short positive read is valid from the file-format contract rather than from the I/O request size.

**Confidence:** Extremely high. Merged master correction authored by Guy Harris.
