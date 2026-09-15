# Automated MR review ledger: !19590-!19639

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Reviewed exactly 50 previously unreviewed merge requests in descending order after consulting the existing per-run ledgers and `reviewed-mrs.md`. The historical !17571-!17620 batch remains part of the already-reviewed set. Because the corpus commit is unchanged from the preceding runs, no newly populated higher-numbered corpus holes displaced this batch.

## Exact reviewed set

| MR | Review status | Notes |
|---|---|---|
| !19639 | Deep | Merged Gerald Combs CMake warning change. Later John Thacker feedback records a compatibility regression with MSVC before 17.10: warning-policy changes must account for the oldest supported compiler's accepted suppression/intent idioms. |
| !19638 | Discussion-focused (closed) | MACsec protocol-column proposal. Jaap Keuter rejected showing selected lower-layer encapsulations in the packet-list protocol column as ambiguous/non-scalable; John Thacker showed a much smaller implementation if pursued. Closed unmerged, so negative UI-design evidence only. |
| !19637 | Discussion-focused (open snapshot) | Password echo-mode hardening. Review raised usability/accessibility tradeoff and preference/reveal option. Unmerged in snapshot, so provisional only. |
| !19636 | Deep | Merged pcapng specification-alignment update. Review explicitly investigated authoritative/current specification provenance rather than accepting an arbitrary draft URL; corroborates source-provenance guidance. |
| !19635 | Scanned | Merged PROFINET ProfiDrive padding correction; narrow protocol-layout fix, no additional durable convention extracted. |
| !19634 | Scanned | ZigBee Green Power data-pointer propagation follow-up; same functional issue as nearby predecessor, no new general rule extracted. |
| !19633 | Scanned | Earlier ZigBee Green Power data-pointer propagation attempt; superseded by later handling, down-weighted. |
| !19632 | Scanned | CIGI field-registration backport/follow-up; no additional lesson beyond registration completeness. |
| !19631 | Scanned | Merged CIGI missing-field registration fix; corroborates existing registration-completeness checks. |
| !19630 | Scanned | Automatic data/translation update; no durable review lesson. |
| !19629 | Scanned | Automatic data/translation update; no durable review lesson. |
| !19628 | Scanned | Automatic data/translation update; no durable review lesson. |
| !19627 | Scanned | Zigbee epoch calculation correction; narrow time-conversion fix, no new convention beyond explicit epoch semantics. |
| !19626 | Scanned (closed) | IEEE 802.11 initializer fix attempt; closed/superseded and therefore down-weighted. |
| !19625 | Deep | Merged Diameter request/reply correlation redesign. John Thacker rejected per-ID transport conversations as unnecessary memory overhead when the protocol already had a multimap and transport tuples are not stable under SCTP multihoming/load sharing. Added durable state-identity guidance. |
| !19624 | Discussion-focused | Windows/MSVC build failure report for an initializer; useful portability signal, but superseded by accepted correction. |
| !19623 | Scanned | O-RAN FH CUS expert-info item correction; narrow tree-parent fix. |
| !19622 | Scanned | Diameter Ciena dictionary corrections/additions; data update without a new coding convention. |
| !19621 | Scanned | Homebrew Qt 5 CMake path handling; build-environment maintenance. |
| !19620 | Scanned | RDP string-encoding fix; corroborates existing encoding-aware string extraction guidance. |
| !19619 | Scanned | Sysdig event zero-data handling; reinforces honoring legitimate zero-length records. |
| !19618 | Scanned | ICMPv6 stat-tap memory-management fix; part of the broader stat-tap lifecycle cleanup represented more strongly by !19601. |
| !19617 | Scanned | ICMP stat-tap memory-management fix; same lifecycle cleanup family. |
| !19616 | Scanned | Dissector-table integer entries displayed using configured base; presentation consistency fix. |
| !19615 | Scanned | GSM A-stat tap reset/finish/error-path cleanup; corroborates stat-tap lifecycle standardization. |
| !19614 | Scanned | Diameter AVP tap removes redundant stored filter string; avoid duplicating state already owned by registration infrastructure. |
| !19613 | Scanned | Merged Commsignia Capture Protocol dissector; no additional durable reviewer convention extracted in this pass. |
| !19612 | Scanned | I/O Graph model/dialog separation; architecture refactor toward generalized graphs, no stronger reusable rule than existing separation-of-concerns guidance. |
| !19611 | Scanned | DCCP comment typo; no engineering lesson. |
| !19610 | Scanned | BGP reader-offset correction; corroborates offset/consumption bookkeeping guidance. |
| !19609 | Scanned | Sysdig data-source naming correction; presentation-only. |
| !19608 | Scanned | Stratoshark suppresses irrelevant local-capture permission warning; product-specific UI behavior. |
| !19607 | Scanned | Release version bump; no engineering lesson. |
| !19606 | Scanned | Release build update; no engineering lesson. |
| !19605 | Scanned | Qt packet-list resize-all-to-contents feature; UI-specific, no durable convention extracted. |
| !19604 | Scanned | Release preparation; no engineering lesson. |
| !19603 | Scanned | Flow stat-tap reset/finish cleanup; same lifecycle standardization family as !19601. |
| !19602 | Scanned | Export-object stat-tap reset/finish cleanup; same lifecycle standardization family as !19601. |
| !19601 | Deep | Merged stat-tap initialization standardization. Callbacks return success, normal error reporting is used, and process termination belongs to the stat-tap UI/application layer rather than individual taps. Martin Mathieson also requested project-defined exit codes. Added durable lifecycle guidance. |
| !19600 | Scanned | Diameter AVP stat-tap reset/finish cleanup; same lifecycle standardization family. |
| !19599 | Scanned | Credentials tap moves global credential array into per-listener state; reinforces explicit ownership/lifetime and avoiding unnecessary globals. |
| !19598 | Scanned | Merged IEEE 802.11 uninitialized-field fix found by scan-build; reinforces static-analysis-driven initialization hygiene. |
| !19597 | Scanned | Silabs debug-channel Railtest support/formatting; protocol-specific feature. |
| !19596 | Scanned | CAMEL SRT tap reset/finish cleanup; same lifecycle standardization family. |
| !19595 | Scanned | ADB null check prompted by Coverity; corroborates static-analysis defensive checks. |
| !19594 | Scanned | EtherType registry addition/backport; data update. |
| !19593 | Scanned | EAX avoids forming an out-of-bounds array-element address, prompted by Coverity; reinforces avoiding undefined/invalid pointer formation even when not dereferenced. |
| !19592 | Scanned | PROFINET additional MAU types; protocol registry/data update. |
| !19591 | Scanned | PCEP SR-PCE-CAPABILITY flag-mask correction; protocol-specific bitmask fix. |
| !19590 | Scanned | Merged widening of loop variables after static-analysis warning; loop/index types must represent the iteration bound. Also replaces a byte-at-a-time copy loop with `tvb_memcpy` where appropriate. |

## Durable notebook changes from this batch

- Added `state-and-lifecycle-conventions.md` with two merged-master rules:
  1. Choose state/correlation identity from protocol semantics rather than forcing state through transport conversations; use a protocol-owned map when transport tuples are unstable or add needless per-conversation overhead (!19625).
  2. Low-level initialization/tap code should return failure and clean up; application/UI ownership layers decide fatality, presentation, and canonical process exit status (!19601).
- !19639 adds useful compatibility evidence: enabling compiler warnings as errors must be tested against the oldest supported compiler behavior, not only the current compiler.
- !19636 corroborates the existing source-provenance rule by explicitly reviewing which pcapng specification source was authoritative/current.

Notebook convention commit preceding this ledger: `d437d3ab4c89e51192a17eee7c78bd4baff0aaa7`.
