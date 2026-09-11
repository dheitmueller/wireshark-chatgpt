# Wireshark Preference State-Transition Conventions

This file records durable conventions for applying preferences whose effects can alter dissection or other live application state. Current upstream implementation remains authoritative.

## Determine effects and quiesce dependent consumers before applying mutations

A preference-application path should not mutate dissection-visible state while it is still discovering whether the resulting change requires redissection or another disruptive transition. If one traversal both applies new values and accumulates effect flags, consumers can observe a mixture of new preference state and old packet-derived state before the application has frozen or rebuilt the dependent views.

Merged master MR !25841, authored by Pascal Quantin and merged by Anders Broman, fixes this ordering in the Qt preferences dialog. The accepted implementation first walks the stashed preferences to compute `redissect_flags`, freezes the packet list when dissection is affected, and only then applies the stashed values. Merged release-4.6 MR !25857 carries the same fix to the stable branch.

Merged follow-up !25862 exposes an important extension of the rule: `rsaKeysFrame->acceptChanges()` also contributes to `redissect_flags`, so it must run early enough that the complete effect set is known before the freeze decision. It is not sufficient to split the main preference traversal if an auxiliary panel can still discover a dissection-changing mutation afterward.

**Implementation rule:** structure preference application as a staged transition: first determine the complete set of effects, including auxiliary preference panels; next quiesce/freeze any consumers that must not observe an intermediate state; then commit the mutations and perform the required redissection/reload. Avoid helpers that hide both effect discovery and externally visible mutation when ordering matters.

**Confidence:** Very high. Merged master correctness fix, stable-branch propagation, and a merged follow-up correcting an omitted effect source.

## Preserve the allocator and lifetime of persistent preference storage

When preference storage is owned by a Wireshark memory scope, replacement code must use the same allocator family and lifetime. Do not free a `wmem`-owned pointer with GLib `g_free()` or replace it with a `g_`-allocated string merely because the value is exposed through a plain `char *`.

Merged MR !23898, authored and merged by Michael Mann, fixes the Manage Interfaces cache after global preference strings moved to `wmem_epan_scope()`: the old code still called `g_free()` and `qstring_strdup()`, while the accepted fix uses `wmem_free(wmem_epan_scope(), ...)` and `wmem_strdup(wmem_epan_scope(), ...)`.

**Implementation rule:** treat the allocator/scope as part of a preference value's ownership contract. When persistent preference memory changes allocator or scope, audit every mutation and replacement path—not just initial allocation—and keep free/duplicate operations paired with that owner.

**Confidence:** Very high. Merged master correctness fix authored and merged by Michael Mann with the ownership mismatch stated directly in the MR.
