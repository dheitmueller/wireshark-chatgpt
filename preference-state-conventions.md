# Wireshark Preference State-Transition Conventions

This file records durable conventions for applying preferences whose effects can alter dissection or other live application state. Current upstream implementation remains authoritative.

## Determine effects and quiesce dependent consumers before applying mutations

A preference-application path should not mutate dissection-visible state while it is still discovering whether the resulting change requires redissection or another disruptive transition. If one traversal both applies new values and accumulates effect flags, consumers can observe a mixture of new preference state and old packet-derived state before the application has frozen or rebuilt the dependent views.

Merged master MR !25841, authored by Pascal Quantin and merged by Anders Broman, fixes this ordering in the Qt preferences dialog. The accepted implementation first walks the stashed preferences to compute `redissect_flags`, freezes the packet list when dissection is affected, and only then applies the stashed values. Merged release-4.6 MR !25857 carries the same fix to the stable branch.

Merged follow-up !25862 exposes an important extension of the rule: `rsaKeysFrame->acceptChanges()` also contributes to `redissect_flags`, so it must run early enough that the complete effect set is known before the freeze decision. It is not sufficient to split the main preference traversal if an auxiliary panel can still discover a dissection-changing mutation afterward.

**Implementation rule:** structure preference application as a staged transition: first determine the complete set of effects, including auxiliary preference panels; next quiesce/freeze any consumers that must not observe an intermediate state; then commit the mutations and perform the required redissection/reload. Avoid helpers that hide both effect discovery and externally visible mutation when ordering matters.

**Confidence:** Very high. Merged master correctness fix, stable-branch propagation, and a merged follow-up correcting an omitted effect source.
