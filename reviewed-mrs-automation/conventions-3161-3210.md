# Convention Synthesis — Wireshark !3161–!3210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Parser progress and unreachable defaults

Merged master !3207, authored by Guy Harris, fixes an IEEE 802.11 HE Trigger loop where an unknown ranging subtype consumed zero bytes and the caller repeated at the same offset. Merged !3209 then explicitly enumerates valid subtypes, filters unknown values before variant parsing, and treats the helper's remaining default as unreachable.

**Rule:** a repeated packet parser must strictly advance or terminate. Assert an impossible default only after untrusted packet values have been validated out.

## Reassembly identity includes protocol context

Merged master !3206 expands the Bluetooth Mesh upper-transport reassembly key beyond source and SeqZero to include IV index and network-key/IV context.

**Rule:** define reassembly equality from full logical PDU identity, including security epoch, interface, direction, or similar context when short sequence spaces can be reused.

## Keep implementation-only Wiretap APIs internal

Guy-authored merged master !3201 moves new-file SHB/NRB and generated-IDB helpers out of public `wtap.h`, into `wtap-int.h`, and removes their exported package symbols.

**Rule:** public headers and symbols are long-lived compatibility promises. Do not export helpers needed only inside Wiretap.

## Generated dissector source remains authoritative

Merged master !3205 changes Kerberos ASN.1, conformance configuration, template code, and regenerated output together. Review explicitly asked for rebase conflict resolution by regenerating ASN.1 output. A capture and keytab were supplied to exercise the feature.

**Rule:** modify authoritative generator inputs first and regenerate derivatives. Supply the minimal capture and key material needed to reproduce crypto-dependent paths.

## Preserve raw values when display preferences transform fields

Merged master !3202 adds relative SCTP TSN display while retaining raw fields. John Thacker caught a path that passed the relative TSN into analysis logic expecting the raw domain.

**Rule:** keep protocol-state algorithms in the raw wire-value domain; derive preference-controlled display values at the presentation boundary.

## Diagnostics should name the format, not implementation helpers

Guy-authored !3183 and !3185 replace malformed-file error prefixes based on internal function names with stable format/subsystem names.

**Rule:** user-facing diagnostics should describe the data format and violated invariant rather than expose refactor-sensitive helper names.

## Keep MR text synchronized after commit-message amendments

During merged !3188, Guy Harris identified that the MR description had diverged from an amended commit message.

**Rule:** treat the commit message and MR description as separate artifacts and update both when review rationale, examples, or references change.

## Negative evidence: test specifications against implementations

Closed !3171 was superseded after review found the written Thrift Compact Protocol documentation inaccurate for some encodings; the supplied sample did not cover those divergent cases.

**Rule:** when standards and implementations may diverge, validate against real implementations and vectors spanning each distinct encoding class.
