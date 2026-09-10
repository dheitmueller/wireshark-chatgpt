# Wireshark WSLua Diagnostic Representation Conventions

This file records durable conventions for debugger-facing and diagnostic string representations of WSLua objects. Current upstream source remains authoritative.

## Make object string representations concise, identifiable, and safe to inspect

A debugger-facing `__tostring` representation should identify the object and summarize useful state without exposing implementation addresses, dumping arbitrarily large content, or becoming unusable when the object is closed/expired.

Merged MR !24474 standardizes WSLua representations across a broad set of classes. The accepted forms use a `Class:` prefix, concise `key=value` state, symbolic names instead of raw numeric IDs where practical, quoted free text, and explicit degenerate-state markers such as `(expired)`, `(closed)`, `(null)`, or `(root)`. The change removes raw-pointer output from classes such as `Conversation`, avoids returning full TextWindow contents as the object's identity, and deliberately bypasses normal validity-check helpers where necessary so an expired/closed object remains inspectable rather than raising while the debugger tries to render it.

**Implementation rule:** design `__tostring` as a compact diagnostic identity, not as serialization or a memory dump. It should remain single-line and useful in debugger variable/watch views, expose stable semantic state rather than pointer values, avoid unbounded payload/text output, and render invalid/expired lifecycle states explicitly without requiring the debugger to dereference an object that is no longer usable.

**Confidence:** High. Large merged master WSLua cleanup authored by Stig Bjørlykke and accepted by Anders Broman; the convention is applied consistently across many object classes.
