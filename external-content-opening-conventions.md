# External Content Opening Conventions

This file records trust-boundary conventions for handing packet-derived or captured content to operating-system viewers and URL handlers.

## Prefer inert handling for packet-derived URLs

Merged master MR !2074, authored by Gerald Combs, changes protocol-tree URL activation from opening the URL through the system handler to copying it to the clipboard. The WSLua browser-opening APIs gain an explicit warning that caller-provided URLs should be trusted before they are passed to a platform handler.

**Security rule:** packet-derived URLs are external input. Prefer an inert action such as copy-to-clipboard over automatic opening, and document the trust boundary on APIs that deliberately invoke an external handler.

**Confidence:** Very high. Merged master hardening authored by Gerald Combs.

## Preview exported objects only after declared type and inspected content agree with a conservative allowlist

The same !2074 change restricts Export Objects preview to a small set of simple text/image formats and verifies the saved object's actual MIME content before invoking the desktop viewer. Other content is shown in its containing folder instead.

**Security rule:** a protocol-advertised MIME type alone is not enough to decide that captured content should be opened. Combine protocol metadata with content inspection and a deliberately conservative preview policy; otherwise leave opening under direct user control.

**Confidence:** Very high. Merged master hardening with the active-content boundary documented in the accepted implementation.
