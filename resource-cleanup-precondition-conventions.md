# Wireshark Resource Cleanup Precondition Conventions

This file records durable conventions for pairing acquisition/initialization APIs with cleanup APIs. Current upstream source and dependency API contracts remain authoritative.

## Call cleanup APIs only after the state they require was successfully initialized

A cleanup routine is not automatically valid merely because the surrounding variable is in scope. Some APIs define their cleanup/free operation only for an object or output structure that was successfully initialized by the corresponding producer call.

Merged master MR !13315, authored by John Thacker, fixed leaks in `CompiledFilterOutput` by closing the dead pcap handle and freeing compiled BPF code. The initial fix called `pcap_freecode(&fcode)` after the compile branch regardless of whether `pcap_compile()` succeeded. Merged master MR !13362, also authored and merged by John Thacker, then fixed a crash by moving `pcap_freecode()` into the successful `pcap_compile()` path: libpcap does not guarantee that the `bpf_program` is valid for `pcap_freecode()` after compilation failure.

This regression chain is useful precisely because the intent of !13315 was correct—close every acquired resource—but the cleanup precondition was too broad. `pcap_close(pd)` is appropriate once `pcap_open_dead()` has returned a handle; `pcap_freecode(&fcode)` is appropriate only once `pcap_compile()` has successfully produced compiled code.

**Implementation rule:** pair each cleanup operation with the exact acquisition or initialization event that makes it valid. Track success state explicitly when necessary. Do not infer that all cleanup functions are safe on zeroed, uninitialized, partially initialized, or failed-output structures unless the API contract explicitly guarantees it.

**Review rule:** when adding missing cleanup on error/success paths, review the producer and destructor contracts together. A leak fix can become a crash if teardown is made unconditional without checking whether the corresponding resource ever reached its initialized state.

**Confidence:** Very high. Two merged master MRs by John Thacker form a direct fix/regression/fix sequence and expose the precise dependency-API precondition.