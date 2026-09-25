# Findings for !9763–!9812

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs, descending from !9812 through !9763. Forty-eight were merged; !9799 was open and !9778 was closed/unmerged, so those two were down-weighted.

## Durable conventions

### Reassembly state should reflect reassembler progress

Merged !9809, authored and merged by John Thacker, distinguishes sender-side TCP retransmission analysis from the reassembler's own view of whether bytes have already been delivered to a subdissector. With out-of-order reassembly enabled, the accepted code tracks the already-consumed prefix separately so repeated bytes can be skipped without losing additional new bytes.

Merged !9812, also authored and merged by John Thacker, clears a request for whole-segment reassembly only when the corresponding reassembly result actually exists.

**Rule:** make reassembly decisions from what the reassembler has accepted and completed, not solely from transport retransmission labels. Retire a pending reassembly condition only after the event that satisfies it.

### Bitmask-array fields should share the underlying scalar width

Merged !9789, authored and merged by Martin Mathieson, extends the typed-item checker to compare the registered widths of fields used together in one bitmask array. The stronger check exposed real width mismatches and led to corrections in existing dissectors.

**Rule:** fields interpreted as parts of one bitmask container should agree on the container width even when their masks occupy only a few low bits. If a checker cannot resolve a width safely, leave it unresolved rather than guessing.

### Shared non-UI helpers belong in a common layer

Merged !9780, authored and merged by João Valverde, moves generic command-line and filter-file helpers from `ui` to `wsutil`, allowing dumpcap and other non-UI programs to use them without linking the UI library.

Open !9799 provides lower-weight corroboration: Gilbert Ramirez objected to duplicating substantial command-line listing code between tshark and the Qt frontend and suggested common ownership.

**Rule:** place reusable non-graphical behavior in the lowest common layer that naturally owns it. Repeated copies across frontends are a signal to look for a shared helper.

### Name local values for their protocol meaning

Merged !9771, authored and merged by Guy Harris, replaces generic TLS local-variable names based on one later use with names for the actual protocol values, such as client version, server version, cipher suite, and supported group.

**Rule:** name a variable for the value it contains, not for one downstream calculation or presentation that happens to consume it.

## Additional evidence

- !9798, with John Thacker review, reinforces that fixed fields must be deducted from a remaining-length value before looping over variable entries; a representative capture was supplied.
- !9776 reinforces matching retained state to file lifetime and storing first-pass results for redissection.
- !9792 followed by !9793 shows that a build-system design that works on one platform still needs validation across the supported build matrix before it is considered complete.
- !9794 contains João Valverde review explaining that Windows build detection must reflect the toolchain/dependency combinations the project actually supports rather than implying support for configurations the dependency model does not handle.
- !9801 replaces one global `-j/-J` mode with per-selector flags, a useful example of attaching semantics to each selector instead of one global setting.
- !9769 and !9770 harden XML output by accounting for maximum escape expansion and XML 1.0 character rules.
- !9763 extends Sharkd JSON output and updates tests, reinforcing that Sharkd machine output is a tested API surface.
- Closed !9778 was superseded by later sshdump work and is not treated as accepted implementation evidence.

## Status inventory

- Merged: !9812–!9800 except none; !9798–!9780; !9779; !9777–!9763.
- Open/down-weighted: !9799.
- Closed/unmerged/down-weighted: !9778.

The exact per-MR membership is authoritative in `reviewed-mrs-automation-9763-9812.md`. The next corpus MR, !9762 (`CoAP: properly dissect OCF version options`), was checked only as the next-frontier probe and is not part of this review.
