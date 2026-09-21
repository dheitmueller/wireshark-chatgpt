# Wireshark Protocol-State Dispatch Conventions

This file records durable conventions for using protocol state to control parsing and dispatch. Current upstream source remains authoritative.

## Explicit protocol state outranks lexical heuristics

A generic "looks like a command/message" test is only valid in states where the protocol grammar actually permits that class of message. Once an earlier exchange has established that the next payload has a particular semantic role, do not run a broader lexical heuristic that can accidentally reinterpret ordinary payload bytes as control syntax.

Merged master MR !14668 fixes SMTP AUTH password decoding. With the SMTP decryption preference enabled, the dissector exposed the cleartext password before deciding how to classify the line. `line_is_smtp_command()` could return true when a password happened to begin with four alphabetic ASCII characters followed by a space, so valid password data was misclassified as an SMTP command. The accepted fix skips that generic command test while the authentication state is `SMTP_AUTH_STATE_PASSWORD_REQ`. The contributor supplied a focused PCAP reproducer and explained why the bug was masked when the same password remained Base64-encoded.

**Dissection rule:** use the protocol state machine to narrow the valid grammar before applying content-based heuristics. Heuristics are fallback evidence for ambiguous contexts, not a reason to override a state transition that already determines what the bytes represent.

**Testing rule:** when a heuristic recognizes a lexical pattern, include payload values that deliberately resemble that pattern while occurring in a state where it must not apply. Test both encoded and decoded/transformed representations when the transformation changes the lexical domain seen by the heuristic.

**Confidence:** High. Merged master correctness fix with a concrete capture reproducer and a clearly described state-dependent false positive.
