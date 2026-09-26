# Wireshark IPC Endpoint Lifecycle Conventions

This file records durable conventions for temporary IPC endpoints and shutdown wakeups.

## Create the final IPC node directly inside a dedicated temporary directory

Merged master MR !9067, authored by Gerald Combs, replaces the extcap sequence of creating a temporary regular file, unlinking it, and creating a FIFO at the same pathname. The accepted implementation creates a dedicated temporary directory and then creates the FIFO inside it. The MR was motivated by a real race with antivirus software that could inspect the first object and later interact with the different object that replaced it.

**Implementation rule:** when the final object is a FIFO, socket, or other special IPC node, allocate a private temporary directory first and create the final object directly inside it. Avoid pathname reuse across object types, and treat the containing directory as part of endpoint cleanup.

## A shutdown wakeup must not itself block

Merged MR !9077 tries to release dumpcap when an extcap process never opens its FIFO by opening and closing the FIFO during shutdown. Gerald Combs then reproduced a macOS hang because opening the writer can itself block when no reader exists. Merged corrective MR !9081 adds `O_NONBLOCK`.

**Implementation rule:** teardown or wakeup I/O must be safe when the peer never reached its expected open/read state. In particular, a FIFO open used only to wake another component should be nonblocking unless peer readiness is guaranteed independently.

**Testing rule:** exercise shutdown with the peer missing, failed before open, already closing, and operating normally.

**Confidence:** Extremely high. Both sequences are merged master work, with the shutdown flaw reproduced and corrected by Gerald Combs.
