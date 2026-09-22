# Wireshark Configuration Reread Conventions

This file records durable conventions for small mutable configuration resources whose contents can change during a Wireshark process lifetime. Current upstream source remains authoritative.

## Prefer on-demand rereads when cache invalidation would be more complex than the read

A small configuration file that can change through several independent paths is often safer to read when needed than to keep in a process-lifetime static cache. This is especially true when changes can come from another UI path, a profile switch, or another running Wireshark instance that cannot participate in the current process's notification graph.

Merged master MR !13122, authored and merged by João Valverde, changes saved filter lists from a static eagerly loaded representation to dynamic on-demand reads. In review, Guy Harris explicitly asked which mutation sources had to be considered and observed that, if notifying every interested in-process consumer would require ugly cross-component plumbing, repeatedly reading a small saved-filter file is probably the right answer because the lists are not large enough for the I/O cost to matter.

**Architecture rule:** choose caching based on invalidation complexity as well as read cost. If a resource is cheap to parse and can be mutated outside the cache owner's observable lifecycle, a fresh snapshot per operation can be a simpler and more correct contract than a long-lived cache plus incomplete notification machinery.

**API rule:** APIs exposing such resources should make snapshot ownership/lifetime explicit. Avoid a static global list whose apparent stability encourages callers to retain stale pointers across profile changes or subsequent edits.

**Review rule:** before caching user configuration, enumerate every mutation source: the same dialog, another component in the process, profile changes, external editors, and other running instances. If the cache cannot reliably observe those sources, either define an explicit staleness contract or prefer rereading.

**Confidence:** Extremely high. Merged master lifecycle/API change plus direct architectural guidance from Guy Harris.
