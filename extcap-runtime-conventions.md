# Wireshark Extcap Runtime Conventions

This file records durable extcap runtime and lifecycle conventions extracted from accepted upstream Wireshark changes. Current upstream source remains authoritative.

## Treat normal control-channel closure as lifecycle, not an error

Extcap control pipes are expected to close during normal teardown. EOF or the platform's normal broken-pipe indication should end the watch quietly; it should not be promoted to a user-facing warning merely because the same API also reports genuine I/O failures.

Merged MR !26123, authored, approved, and merged by John Thacker, fixes the Windows/MSYS2 extcap control path so `ERROR_BROKEN_PIPE` is logged at debug level and ends the read, while user-facing error reporting is reserved for a negative read result carrying a real error message.

**Implementation rule:** classify control-channel termination according to its lifecycle semantics. Normal peer closure should stop the source cleanly; reserve warnings/errors for unexpected failures that require user attention.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker.

## Remote-capture read loops must remain interruptible even when no packets arrive

A long-lived extcap loop cannot rely on incoming capture data to regain control. A valid capture filter may suppress every packet indefinitely, so shutdown must be represented as an independent readiness event that can wake the loop even when the remote command produces no stdout or stderr.

Merged MR !26131, authored by John Thacker and approved/merged by Anders Broman, changes `sshdump`/`ssh-base` to use `ssh_select()` rather than a blocking channel read and, on non-Windows systems, includes a self-pipe in the same wait set. The graceful-shutdown callback writes to the pipe, waking the loop; teardown can then explicitly signal the remote SSH command instead of depending on data arrival. The MR also documents why allocating a PTY is not an acceptable workaround for binary capture data: it can merge stdout/stderr and transform bytes.

**Implementation rule:** integrate shutdown signaling into the same readiness mechanism used for remote I/O. Do not let an idle producer make an extcap process unkillable, and do not use terminal semantics on a binary capture channel merely to obtain signal behavior.

**Confidence:** Very high. Merged master lifecycle/architecture change authored by John Thacker and approved/merged by Anders Broman.

## Prime asynchronous I/O before attaching the readiness source

When asynchronous I/O needs an initial read operation to establish its pending/event state, complete that priming step before publishing the event source to the main loop. Attaching the source first can let immediately available data trigger the callback while the explicit priming path is also consuming it, producing duplicate callbacks or inconsistent state.

Merged MR !26146, authored, approved, and merged by John Thacker, fixes the Windows extcap control path by calling `issue_next_read()` before `g_source_attach()`. The source is attached, and its watch ID stored, only when the OVERLAPPED read has been successfully primed; failed priming leaves the watch unset.

**Implementation rule:** initialize the platform I/O operation and establish its one authoritative pending/readiness state before exposing the corresponding event source to the main loop. Publish the watch handle only after successful priming.

**Confidence:** Very high. Merged master fix authored and merged by John Thacker, with an explicit duplicate-callback failure mode.

## Teardown should remove auxiliary IPC resources as completely as setup created them

Extcap sessions can create more than the main capture FIFO. Control input/output FIFOs may each live in temporary directories, and removing only the FIFO pathname leaves process-generated filesystem state behind.

Merged MR !26157, authored, approved, and merged by John Thacker, extends non-Windows extcap teardown so the control FIFO paths are unlinked and their temporary parent directories are removed just like the main FIFO directory.

**Implementation rule:** treat session-owned IPC artifacts as a resource set. Teardown should mirror setup for main and auxiliary channels, including temporary container directories, while retaining platform-specific behavior where required.

**Confidence:** Very high. Merged master lifecycle cleanup authored and merged by John Thacker.