# Dissector default-activation conventions

## An unconfigured default must not become a catch-all

Audit masks, wildcard ranges, and selector defaults so that absence of user configuration does not cause a dissector to claim an entire parent protocol domain.

Merged !2356 fixes AUTOSAR NM's default CAN-ID mask. A zero mask made every comparison succeed, so an unconfigured dissector attempted to dissect all CAN packets. The corrected all-ones mask limits the default match.

**Implementation rule:** test the exact no-configuration state of masked/ranged selectors and prefer conservative defaults that cannot accidentally match everything.

**Confidence:** High. Merged master correctness/performance fix with a clear failure mode.
