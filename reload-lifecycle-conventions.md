# Wireshark Reload Lifecycle Conventions

This file records durable ownership rules for state that can be recreated during plugin, configuration, interpreter, or subsystem reload. Current upstream source remains authoritative.

## Release the previous owned container before replacing it during reload

A registration or initialization routine that can run more than once must treat replacement of process/static state as an ownership transition. Reassigning a static pointer to a newly allocated container without destroying the old one leaks the old container and can leak or orphan any elements it owns.

Merged master MR !15745, authored and merged by John Thacker, fixes WSLua plugin reload leaks across several static `GPtrArray` and `GHashTable` objects. The registration functions can be invoked again when plugins are reloaded; the accepted implementation checks for an existing container and unreferences it before constructing and assigning the replacement.

**Implementation rule:** for reloadable registration/state, assume initialization can repeat. Before replacing an owned static/global container, run the old container's correct teardown path (`g_ptr_array_unref()`, `g_hash_table_unref()`, or the relevant project ownership routine), then install the new state. Prefer containers with destroy callbacks when element ownership can be expressed directly.

**Review rule:** when a reload/reinitialize path allocates static or subsystem-owned state, compare the first-run and subsequent-run ownership transitions explicitly. A function named `*_register()` or `*_init()` is not necessarily one-shot in Wireshark; plugin reload, profile changes, and interpreter reset can cause valid repetition.

**Confidence:** Extremely high. Merged master leak fix authored and merged by John Thacker, covering multiple independent WSLua static containers.