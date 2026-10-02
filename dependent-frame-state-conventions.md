# Wireshark Dependent-Frame State Conventions

This file records durable conventions for frame-dependency bookkeeping discovered while reviewing Wireshark merge requests. Current upstream source remains authoritative.

## Treat dissection-derived dependencies as redissection-sensitive set state

A frame's dependency list is derived from the current dissection, including reassembly behavior and preferences. It is therefore not immutable capture metadata. If frame data is reset for redissection, stale dependency state must be cleared as well.

Merged release-4.0 MR !9894, authored by John Thacker, prevents the same dependent frame from being inserted more than once, avoids recursively marking frames already known to be displayed/dependent, and clears `dependent_frames` when frame data is reset because a later dissection can discover a different dependency graph. Merged follow-up !9895 replaces the list with a direct-key hash table because dependency order is irrelevant and list membership checks become expensive for reassemblies with many fragments.

**Architecture rule:** model frame dependencies as a set whose lifetime follows the dissection-derived frame state. Clear/rebuild that set at the same reset boundary as the dissection state that produced it.

**Data-structure rule:** when only membership/uniqueness matters and order does not, use set/hash semantics rather than a list plus repeated linear duplicate searches. This matters especially for fragment-heavy captures where the dependency graph can become large.

**Testing rule:** exercise redissection after a preference/state change that can alter reassembly, and exercise captures with many contributing fragments. Verify both that stale dependencies disappear and that filtered save/export still retains the required dependency closure.

**Confidence:** Very high. Two consecutive merged release-4.0 core changes authored by John Thacker establish both the state-lifecycle and set/performance aspects, and they align with later notebook guidance on transitive reassembly dependencies.
