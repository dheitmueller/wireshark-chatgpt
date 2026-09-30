# Review findings: Wireshark MRs !3611–!3660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is accepted precedent; closed work is lower-weight history. Maintainer-authored/reviewed changes, especially Guy Harris, receive greatest weight.

| MR | State | Finding |
|---|---|---|
| !3660 | merged | Guy Harris naming cleanup for the capture-comment option constant. |
| !3659 | merged | Guy Harris orders pcapng option helpers to mirror the option-type enum. |
| !3658 | merged | Fixes ISIS flexible-algorithm spelling/filter names. |
| !3657 | merged | Guy Harris aligns Wiretap pcapng option naming/order with the specification. |
| !3656 | merged | Reduces noisy Windows CI output. |
| !3655 | merged | Guy Harris: packet flags can be present with value zero; zero is not absence. Packet record type and block type must agree. |
| !3654 | merged | Targets must declare direct CMake requirements instead of relying on accidental transitive includes; PUBLIC/PRIVATE is an interface boundary. |
| !3653 | merged | Release preparation only. |
| !3652 | merged | Release preparation only. |
| !3651 | merged | Corrects rpcap/Npcap documentation to current capability. |
| !3650 | merged | Marks external Zlib includes SYSTEM. |
| !3649 | merged | Adds DCT2000 logged NR-MAC support. |
| !3648 | merged | No-padding 802.11 trigger frames must not be called malformed; gate trailing parsing on remaining bytes. |
| !3647 | merged | Removes very old PPCAP obsolete-preference scaffolding. |
| !3646 | merged | Guy Harris: comments apply to all pcapng blocks, so process them once in common option code. |
| !3645 | merged | Guy Harris: Windows-only code needs Windows MR CI because other platforms may not compile it at all. |
| !3644 | merged | New BLF reader includes representative samples; reviewer requests release-note entry. |
| !3643 | merged | EPB flags move to typed packet-block options; option existence carries presence. |
| !3642 | merged | Accepted DPoE parsing fix. |
| !3641 | closed | Superseded by !3642; lower-weight history. |
| !3640 | merged | Guy Harris distinguishes a no-libpcap CI failure caused by another MR from the current Qt change. |
| !3639 | merged | Adds optional multipart gzip decompression under Zlib. |
| !3638 | merged | Adds obsolete-only protocol preference registration for legacy-key recognition. |
| !3637 | merged | Typed-item checker typo fix. |
| !3636 | merged | Moves generic wmem infrastructure to wsutil; EPAN keeps scope lifecycle, and scope-validity assertions are retained after review. |
| !3635 | merged | LIN support uses carrier adapters feeding common ISO15765 semantics; Jaap requires editing release-note source, Guy requests an authoritative LINKTYPE_LIN standard reference. |
| !3634 | merged | Checker finds masks wider than registered integer types and incorrect boolean width metadata. |
| !3633 | merged | Automated data update. |
| !3632 | merged | Guy Harris: sharkd API distinguishes no-frame/read-error/success and allows caller-owned reusable record/buffer state. |
| !3631 | merged | Automated data update. |
| !3630 | merged | Automated data/translation update. |
| !3629 | merged | Fixes wsutil format_size and adds wsutil unit tests. |
| !3628 | merged | HTTP/2 and QUIC Follow Stream carry logical stream identity in tap data and filter at the protocol tap; focused multistream captures/tests added. |
| !3627 | merged | Author metadata update. |
| !3626 | merged | Author metadata update. |
| !3625 | merged | Stable NULL-guard backport. |
| !3624 | merged | Stable NULL-guard backport. |
| !3623 | closed | Gerald Combs: backport is inapplicable because the 3.2 structure lacks the referenced member. |
| !3622 | merged | DCERPC initialization fix from Clang Analyzer. |
| !3621 | merged | Uses MAX_TIMESTAMP_LEN for DCT2000 timestamp storage. |
| !3620 | merged | Stable counterpart of !3621. |
| !3619 | merged | Snort config error path frees temporary path allocations. |
| !3618 | merged | Stable counterpart of !3619. |
| !3617 | merged | wmem allocation must use matching wmem free family; packet-pool lifetime often removes need for explicit free. |
| !3616 | merged | Stable counterpart of !3617. |
| !3615 | merged | S101 sets Protocol column only after positive header recognition, avoiding false labeling of TCP/9000. |
| !3614 | merged | LDAP OID change updates ASN.1 template and generated C together. |
| !3613 | merged | PROFINET loop keeps searching until the intended submodule is found. |
| !3612 | merged | Stable counterpart of !3613. |
| !3611 | merged | OSPF RFC 8665 TLV correction. |

## Strongest durable evidence

The strongest items are Guy Harris's !3655 present-zero-vs-absent metadata rule, !3646 common pcapng option layering, direct !3645 platform-CI guidance, and !3632 sharkd API cleanup. !3636 is strong library-layering evidence, and !3628 is a strong logical-substream/tap design exemplar with targeted captures and tests. !3654 adds useful direct-dependency evidence, while !3635 and !3615 reinforce authoritative specification provenance and conservative protocol claiming.

No SMPTE 291/VANC packet type was encountered in this batch.
