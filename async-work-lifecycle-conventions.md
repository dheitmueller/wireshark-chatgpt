# Wireshark Asynchronous-Work Lifecycle Conventions

This file records durable conventions for asynchronous work whose results are consumed across analysis phases or mode changes. Current upstream source remains authoritative.

## Drain in-flight asynchronous work before a synchronous consumer assumes completion

Changing a subsystem from asynchronous to synchronous operation is a synchronization boundary, not just a boolean configuration change. Work already queued or in flight under the old mode must be brought to a state consistent with the guarantees expected by the new mode before callers continue.

Merged master MR !14244, authored by John Thacker and approved/merged by Anders Broman, fixes external host-name resolution when TShark switches from its first asynchronous pass to its second synchronous pass. An address could already be marked in the cache as having a request outstanding; without waiting for that request, the second-pass synchronous lookup saw the marker, immediately reported failure, and never obtained the result in time for command-line output. The accepted implementation processes queued requests and waits for the async c-ares work to drain when synchronous resolution is enabled.

**Architecture rule:** define explicit quiescence semantics at async-to-sync, producer-to-consumer, and phase boundaries. Before the consumer relies on a cache/state as complete, either wait for outstanding producers or cancel them and repair their state deliberately. A flag saying “synchronous now” does not retroactively make previously submitted asynchronous work complete.

## Make an asynchronous phase actively service the queue it is intended to hide latency for

Queuing work asynchronously is useful only if the phase that can overlap that work also services the queue and collects results. Deferring queue processing until the phase boundary defeats the latency-hiding design and can force the next phase to wait synchronously for work that could already have completed.

Merged master MR !14248, authored and merged by John Thacker, complements !14244 by having the first pass of two-pass TShark process queued asynchronous DNS lookups as packets are processed. The accepted code also protects queue removal with a mutex because the queue can be serviced from more than one execution context. The MR notes the longer-term optimization target explicitly: discover second-pass lookup needs early enough that those requests can run during the first pass.

**Performance rule:** if a first pass or preparatory phase exists partly to overlap external work, service the async queue during that phase rather than only at its end. Where multiple contexts can dequeue or mutate the work list, make queue ownership/synchronization explicit even if races appear unlikely in the usual frontend.

**Testing rule:** exercise phase transitions with work still queued and work already in flight. For two-pass analysis, verify not only that eventual GUI updates occur, but that non-interactive consumers have the required results at the point their second pass renders or filters output.

**Confidence:** Very high. Both changes are merged master fixes authored by John Thacker; !14244 was approved/merged by Anders Broman and !14248 was authored/merged by John, and the two MRs directly describe the broken phase-boundary semantics.