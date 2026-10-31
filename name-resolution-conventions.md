# Wireshark Name-Resolution Conventions

This file records durable conventions for DNS/name-resolution behavior across Wireshark frontends. Current upstream source remains authoritative.

## Match blocking versus asynchronous resolution to the frontend lifecycle

Name-resolution policy should follow the execution model of the frontend rather than being copied mechanically from another program. A single-pass streaming CLI can reasonably prefer synchronous resolution when it needs names before emitting final output. A stateful interactive frontend or daemon can instead resolve asynchronously and expose newly available names on later redissection or client requests; forcing each request to wait for external DNS can destroy responsiveness without adding useful correctness.

Merged MR !23110 fixes sharkd after an earlier change had made external DNS resolution synchronous in order to behave more like TShark. During review John Thacker explicitly challenged the premise because the synchronous behavior had been intentional and asynchronous resolution does not itself signal clients to retry. Zach Chadwick explained the architectural distinction: sharkd retains captures in memory, serves repeated interactive requests, and already allows later-derived information to appear on subsequent requests, unlike TShark's single-pass streaming output. He also reported that synchronous external resolution blocked every lookup and made sharkd practically unusable with name resolution enabled. John accepted that explanation, the discussion was resolved, and the change merged after further maintainer approval.

**Implementation rule:** choose resolver synchronization from the frontend's data and interaction lifecycle. Do not force a stateful interactive service to block every analysis request merely to imitate a streaming CLI; if the frontend naturally revisits retained data, permit asynchronously resolved names to become visible on later requests or redissection. Conversely, do not assume asynchronous behavior is automatically correct for a one-pass producer that cannot revise output.

**Confidence:** Very high. Merged master architecture/correctness fix with an explicit maintainer challenge, architectural justification, acceptance, and subsequent stable-branch backport (!23124).

## Cache failed resolution attempts only when resolution was actually attempted

Negative lookup state is useful for avoiding repeated expensive work, but it must describe what really happened. A cache entry created while name resolution is disabled must not be marked as an attempted-and-failed resolution, because enabling resolution later would otherwise suppress the first real lookup. At the same time, repeated accesses after an actual failed lookup should not continually reopen resolver data sources or repeat the same work.

Merged master MR !16372, authored and merged by John Thacker, adds a `TRIED_RESOLVE_ADDRESS` distinction so unresolved EUI-48 addresses are not repeatedly resolved on later accesses. The flag is set only when resolution was requested, and the lookup path deliberately preserves precedence for ethers-file results over other resolution sources. The competing draft !16371 used the broader tried-or-resolved mask; John explicitly pointed out that this could mark an address as tried when Hardware Name Resolution was disabled. That draft was closed in favor of !16372.

**Implementation rule:** negative-cache flags must represent an executed operation, not merely observation of an unresolved object. Set “tried” state only when the corresponding resolver was enabled and actually consulted; otherwise later configuration changes must still be able to trigger the lookup.

**Invalidation/precedence rule:** define when negative state is reset and preserve the intended precedence among resolver sources. Avoid broad masks that collapse “resolved elsewhere”, “attempted here and failed”, and “not attempted because disabled” into one state when those cases lead to different future behavior.

**Confidence:** Very high. Merged master implementation authored and merged by John Thacker, contrasted directly with a closed competing draft whose semantic flaw John identified.

## Parsed pcapng Name Resolution Blocks must survive and be replayed on redissection

Merged master MR !9550 (John Thacker) makes IPv4/IPv6 address/name entries mandatory data owned by each `WTAP_BLOCK_NAME_RESOLUTION` rather than treating a one-time parsing callback as their only representation. Wiretap's new NRB processing helper replays accumulated entries whenever a name-resolution callback is registered. In `file.c`, a redissection resets the IPv4/IPv6 callbacks alongside the DSB secrets callback after the prior `epan` context is discarded. The packet-only reread does not necessarily reread NRB records, so without replay names previously present in the file would disappear.

**Architecture rule:** Persist non-packet metadata at the file-reader/block lifetime, replay it when a consumer's cache or dissection context is recreated, and free the owned records with the block. Test names before the first packet, between packets, after packets, and after redissection.

**Scope qualification:** !9550 did **not** implement full NRB deep-copy/serialization for writers (the proposed mandatory-data copy routine remained disabled); do not mistake its replay behavior for a guarantee that an arbitrary capture transform preserves NRBs. Related later accepted NRB work appears in !9573, !9608 and !9669. **Confidence:** Very high for the reader/redissection invariant (merged John Thacker implementation); later MRs qualify output preservation.

## Captured DNS PTR records can populate resolution caches, but only during the authoritative pass

Merged master !9486 (John Thacker) uses DNS `PTR` answers for packet-derived IPv4/IPv6 name resolution as well as forward-answer data. It gates the cache mutation on the user preference `dns_pkt_addr_resolution`, IN-class DNS answer, and `!PINFO_FD_VISITED(pinfo)`, then decodes `in-addr.arpa`/`ip6.arpa` forms into address types before calling `add_ipv4_name()` or `add_ipv6_name()`.

**Rule:** when packet dissection populates a shared name resolver, distinguish first-pass fact collection from redissection rendering and apply the user's resolution preference. Require a valid parsed address before inserting derived records; avoid rebuilding or mutating the resolver through repeated display passes. **Confidence:** high for the gating and accepted master implementation; do not assume this one historical parser handles every modern reverse-DNS edge case.
