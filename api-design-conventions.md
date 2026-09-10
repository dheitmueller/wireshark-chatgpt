# Wireshark API Design Conventions

This file records durable API and internal-interface design conventions extracted from upstream Wireshark merge-request review. Current upstream source remains authoritative.

## Name APIs for their semantic operation, not their current implementation mechanism

An interface name should describe what callers are asking Wireshark to do, rather than expose the library or system call currently used to implement it. This keeps implementation details from becoming part of the conceptual API and makes later implementation changes less disruptive.

Merged MR !24345, authored and merged by Guy Harris, renames interface-enumeration routines whose names were derived from `pcap_findalldevs()` to semantic names such as `get_local_interface_list()` and `get_remote_interface_list_common()`. The callers care about obtaining local or remote interfaces, not which libpcap entry point happens to provide them.

**Implementation rule:** choose function names from the stable caller-visible responsibility. Avoid embedding a specific backend/API mechanism in an internal interface name unless that mechanism is itself part of the contract.

**Confidence:** Very high. Merged architectural/API cleanup authored and merged by Guy Harris.

## Remove parameters and branches that no longer represent real caller choices

Do not preserve a boolean/mode parameter merely because an older implementation once had multiple paths. If every caller supplies the same value and the other path is dead or meaningless, eliminate the parameter and simplify the helper. File-private implementation helpers should also remain `static` rather than accidentally widening linkage.

Merged MR !24342, authored and merged by Guy Harris, simplifies interface-listing support after observing that all callers selected the same mode and the alternate branch could only return an empty result. It removes the constant boolean parameter and makes local helpers file-private where appropriate.

**Implementation rule:** periodically re-evaluate helper signatures after refactoring. A parameter is useful only when callers have a meaningful supported choice; dead configurability obscures invariants and makes maintenance harder.

**Confidence:** Very high. Merged cleanup authored and merged by Guy Harris, with the simplification directly reflected in the accepted code.
