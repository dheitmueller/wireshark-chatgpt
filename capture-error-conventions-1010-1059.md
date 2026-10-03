# Capture and Error Conventions from !1010-!1059

## Validate framing invariants immediately after decoding the controlling field

Merged master MR !1029, authored and merged by Guy Harris, checks pcapng `block_total_length` for the required four-byte multiple immediately after reading it and reports the actual invalid value.

**Rule:** once a framing length is available, check unconditional structure rules such as alignment before deeper processing. Report the violated invariant and offending value when practical.

## Preserve structured error identity across helper boundaries

Merged master MR !1027, authored and merged by Guy Harris, changes `capture_loop_init_pcapng_output()` to forward the numeric error from the actual pcapng write operation to its caller.

**Rule:** if a caller needs the specific failure for correct diagnosis or policy, preserve the structured error code instead of collapsing it to a Boolean.

## Select the error domain from the operation

Merged master MR !1030, authored and merged by Guy Harris, confines `WSAGetLastError()` to Windows socket I/O while pipe I/O uses the ordinary errno path.

**Rule:** retrieve and format errors using the domain defined by the API that failed. The host platform alone does not identify the correct error source.

## Model each capture-source object type before changing open/classification order

Closed MR !1045 proposed changing pipe/socket classification to address a race. Guy Harris points out that `open()` is not guaranteed to work on Unix-domain sockets and that the capture-source privilege boundary should be considered as part of the design.

**Rule:** do not assume files, FIFOs, and sockets share one open/classification contract. This is authoritative review guidance from a closed MR, not merged implementation precedent.
