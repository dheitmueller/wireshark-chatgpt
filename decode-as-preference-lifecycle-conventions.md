# Decode As and preference-lifecycle conventions

Merged master MR 26, authored by John Thacker, fixes a lifecycle bug by calling `prefs_apply_all()` after Decode As changes and before redissection. Stable backport MR 53 corroborates the behavior.

The key lesson is that Decode As can be more than a dissector-table mutation. A protocol preference may feed derived state used elsewhere in the dissector. John documented TFTP as the concrete case: replacing the preference range without applying the protocol preference callback left derived state referring to the old range during redissection.

## Convention

When Decode As or another UI action changes protocol preferences:

1. update the selected preference or dispatch mapping;
2. apply affected preference callbacks and rebuild derived protocol state;
3. only then redisect packets.

Reviewers should look for stale derived state whenever a preference object can be replaced, resized, freed, or otherwise invalidated before redissection.
