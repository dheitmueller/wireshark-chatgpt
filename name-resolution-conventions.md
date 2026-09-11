# Wireshark Name-Resolution Conventions

This file records durable conventions for DNS/name-resolution behavior across Wireshark frontends. Current upstream source remains authoritative.

## Match blocking versus asynchronous resolution to the frontend lifecycle

Name-resolution policy should follow the execution model of the frontend rather than being copied mechanically from another program. A single-pass streaming CLI can reasonably prefer synchronous resolution when it needs names before emitting final output. A stateful interactive frontend or daemon can instead resolve asynchronously and expose newly available names on later redissection or client requests; forcing each request to wait for external DNS can destroy responsiveness without adding useful correctness.

Merged MR !23110 fixes sharkd after an earlier change had made external DNS resolution synchronous in order to behave more like TShark. During review John Thacker explicitly challenged the premise because the synchronous behavior had been intentional and asynchronous resolution does not itself signal clients to retry. Zach Chadwick explained the architectural distinction: sharkd retains captures in memory, serves repeated interactive requests, and already allows later-derived information to appear on subsequent requests, unlike TShark's single-pass streaming output. He also reported that synchronous external resolution blocked every lookup and made sharkd practically unusable with name resolution enabled. John accepted that explanation, the discussion was resolved, and the change merged after further maintainer approval.

**Implementation rule:** choose resolver synchronization from the frontend's data and interaction lifecycle. Do not force a stateful interactive service to block every analysis request merely to imitate a streaming CLI; if the frontend naturally revisits retained data, permit asynchronously resolved names to become visible on later requests or redissection. Conversely, do not assume asynchronous behavior is automatically correct for a one-pass producer that cannot revise output.

**Confidence:** Very high. Merged master architecture/correctness fix with an explicit maintainer challenge, architectural justification, acceptance, and subsequent stable-branch backport (!23124).
