# Windows Console and Standard-I/O Conventions

This file records durable Windows startup and console behavior relevant to capture and logging. Current upstream code remains authoritative.

## Attaching a console must not destroy an inherited capture pipe

A GUI process can need to attach to its parent console so early logging becomes visible, but standard handles may already represent redirected pipes rather than console streams. Console setup must preserve those inherited handles unless the program has explicitly decided to replace them.

Merged master MR !180 moved Wireshark's console and logging initialization earlier in startup. Post-merge discussion then demonstrated that parent-console attachment disrupted stdin for capture-from-stdin. The proposed repair saves the existing standard-input handle before console attachment and restores it when stdin was not one of the streams that needed console redirection; the reporter confirmed that this restored the pipe.

**Implementation rule:** classify each standard stream before attaching or allocating a console. Preserve a stream that already carries redirected input or output, and redirect only the handles that actually require a console endpoint.

**Review/test rule:** Windows startup changes that touch console attachment or standard handles should be exercised with inherited console handles and with redirected stdin, stdout, and stderr, especially capture-from-stdin. Logging initialization must not silently change the capture I/O topology.

**Confidence:** High for the invariant and regression evidence. The original MR merged, but its initial implementation exposed this regression in subsequent discussion, so the first implementation itself is not treated as final precedent.
