# Durable conventions from Wireshark MRs !3611–!3660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Metadata presence is separate from numeric value

Guy Harris's merged !3655 shows that capture metadata which is always present must remain present even when its numeric value is zero. iptrace/Sniffer/Peek packet flags can legitimately be zero, meaning “no errors,” without meaning “flags were not supplied.” Merged !3643 moves these flags into typed packet-block options.

**Rule:** test option/getter success separately from the payload value. Never infer absence merely from zero when the format defines a present zero-valued field.

## Declare direct build dependencies

Merged !3654 exposed targets that had accidentally inherited include paths through wsutil after those paths became PRIVATE. The accepted fix gives each consumer its actual direct includes/libraries.

**Rule:** use CMake PUBLIC/PRIVATE according to the downstream interface contract. Do not rely on unrelated transitive include paths to make a target compile.

## Put universal format semantics in common parsing code

Guy Harris's merged !3646 moves pcapng `OPT_COMMENT` handling into the common options processor because comments apply to all pcapng block types.

**Rule:** when a format rule is universal across block/record variants, implement it once at the common parsing layer rather than duplicating per-variant cases.

## Platform-specific code needs platform-specific MR CI

Merged !3645 fixed Windows-only source that non-Windows builds never compiled. Guy Harris explicitly called out the need for Windows merge-pipeline coverage.

**Rule:** CI must compile code under the platform guards that own it. A green UNIX build is not evidence that Windows-only code compiles.

## Keep generic memory management below EPAN while preserving scope contracts

Merged !3636 moves wmem to wsutil so generic low-level code can use it without an EPAN dependency or duplicated allocator implementation. Review distinguishes the generic allocator from EPAN's file/packet scope lifecycle. Scope-validity assertions were retained after Evan Huus noted they still enforce an important contract.

**Rule:** move generic infrastructure to the lowest common owning layer, but do not discard higher-level lifecycle checks just because the implementation moved. Remove such assertions only when the underlying scope contract itself is eliminated.

## Filter logical streams where logical identity is known

Merged !3628 fixes HTTP/2 and QUIC Follow Stream when one physical packet contains multiple logical streams. The tap payload carries a stream ID and the protocol-specific tap listener filters the selected substream. Focused HTTP/2 and QUIC multistream captures and tests verify the behavior.

**Rule:** if one capture record can contain several logical streams, packet/frame identity is insufficient for Follow Stream. Carry logical substream identity in tap data and filter at the protocol layer that knows it.

## Adapt carrier-specific context into a common semantic core

Merged !3635 extends ISO15765 from CAN-only assumptions to CAN, CAN-FD, and LIN by using separate carrier entry points that normalize bus type, frame ID, and length before calling common logic.

**Rule:** when shared protocol logic runs over multiple carriers, keep carrier-specific structures at adapter entry points and pass a compact semantic context into the shared core and children.

## Give new capture/link types authoritative format provenance

During !3635, Guy Harris asked that LINKTYPE_LIN documentation point to an authoritative standard such as ISO 17987 rather than an inaccessible or obsolete informal specification.

**Rule:** a new link type or capture format should have a durable, authoritative public/specification reference wherever the registry/documentation permits.

## Delay protocol-claim side effects until recognition

Merged stable MR !3615 moves the S101 Protocol-column update until after an S101 header is actually found; otherwise unrelated TCP/9000 traffic is mislabeled.

**Rule:** do not set protocol columns or otherwise claim traffic before the dissector has positively recognized its framing when the dispatch key can carry unrelated traffic.

## Preserve actionable API error categories and reusable loop state

Guy Harris's merged !3632 makes sharkd dissection return distinct success/no-such-frame/read-error status, passes read error detail back to the caller, and lets callers reuse `wtap_rec` and Buffer objects across frame loops.

**Rule:** avoid collapsing materially different failure modes into one generic return. In high-volume loops, let callers own/reuse state whose lifetime naturally spans iterations.

## Secondary corroboration

!3634 is early typed-item-checker evidence that field masks must fit registered integer widths and boolean display-width metadata must match the aggregate field. !3617/!3616 reinforce matching wmem allocation/free families and avoiding unnecessary manual frees for packet-pool lifetime. !3638/!3647 show obsolete-preference compatibility as an explicit migration mechanism that can eventually be retired after its supported compatibility horizon.
