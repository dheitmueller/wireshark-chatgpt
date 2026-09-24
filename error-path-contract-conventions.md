# Wireshark Error-Path Contract Conventions

This file records durable API and wiretap error-path rules extracted from accepted upstream changes. Current upstream source and the notebook's newer wiretap error taxonomy remain authoritative.

## Do not consume success-only output parameters after failure

An output parameter commonly becomes valid only after the operation that owns it succeeds. On an error return it may still contain caller garbage or a value from an earlier operation, so even debug logging can read meaningless state if it uses the output before the contract says it was written.

Merged master MR !10679, authored and approved by Guy Harris, removes BLF error-path logging of `*data_offset` because the failed read had not guaranteed that output was initialized; Guy notes that this is especially visible when the first packet is being read. Release-4.0 and release-3.6 backports !10680 and !10681 preserve the correction.

**API rule:** define which outputs are valid on success and which, if any, are valid on failure. Callers must not inspect, log, free, or branch on a success-only output after a failed call unless the callee explicitly initializes it for failure as part of its contract.

**Confidence:** Extremely high. Merged master error-path fix authored and approved by Guy Harris and carried to two stable branches.

## Preserve error meaning across helper boundaries

A low-level read failure must not change meaning merely because an intermediate helper returns it through a generic status path. In particular, a truncation inside a structure that is already known to be present must not bubble up as an error code that the outer reader interprets as ordinary end-of-file.

Merged master MR !10670, authored and approved by Guy Harris, fixes a BLF path where a short read while loading compressed container data propagated as `WTAP_ERR_SHORT_READ`; the caller interpreted that code as normal EOF and silently lost the error. The historical patch converted it to `WTAP_ERR_INTERNAL`, with the MR itself explicitly noting that the file-format handling needed further rework and better documentation. Release backports !10671 and !10672 retained that immediate correction.

**Error-semantics rule:** preserve the semantic distinction between normal EOF and truncation/failure inside an expected record or container. Translate errors at abstraction boundaries when the callee's code would otherwise be misinterpreted by the caller.

The newer wiretap taxonomy already recorded elsewhere in this notebook remains authoritative for the classification: malformed/truncated input should normally be `WTAP_ERR_BAD_FILE`, while `WTAP_ERR_INTERNAL` is reserved for violated Wireshark invariants. The durable lesson from !10670 is the boundary-semantics bug, not the old choice of `WTAP_ERR_INTERNAL`.

**Confidence:** Extremely high for the error-propagation rule because the master fix was authored and approved by Guy Harris; later notebook evidence intentionally supersedes its temporary error-class choice.
