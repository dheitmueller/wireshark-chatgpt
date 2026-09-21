# Wireshark Native Platform Integration Conventions

This file records durable conventions for platform-specific desktop integration. Current upstream source remains authoritative.

## Prefer the native platform API over spawning a scripting intermediary

When the operating system exposes a direct API for a desktop operation, prefer calling that API from the platform bridge instead of starting a shell or scripting interpreter and constructing source text or command-line arguments to request the same operation. The native path avoids an additional process, reduces quoting and encoding edge cases, and lets the platform framework own the semantic conversion of paths and objects.

Merged master MR !14417 replaces the macOS "Show in Finder" implementation that invoked `/usr/bin/osascript` with AppleScript text with a Cocoa bridge using `NSWorkspace`'s `activateFileViewerSelectingURLs:`. The accepted code converts the UTF-8 path into an `NSString`/`NSURL` and performs the operation directly. Guy Harris approved the MR, rebased it, and enabled the merge, giving the accepted platform-specific design especially strong maintainer weight.

**Implementation rule:** for supported platform integrations such as file-manager reveal/open operations, notifications, or other desktop services, use the stable native API behind a small platform-specific bridge when one exists. Avoid constructing script source or shell command strings merely to reach functionality already exposed by the operating system SDK.

**Review rule:** when replacing a scripted/subprocess path, check filenames with quotes, backslashes, non-ASCII characters, and other characters that previously required escaping, and verify the native API receives the intended platform object rather than a lossy textual approximation.

**Confidence:** Very high. Merged master platform cleanup with explicit approval and merge handling by Guy Harris; the diff directly removes AppleScript escaping/process invocation in favor of the Cocoa API.