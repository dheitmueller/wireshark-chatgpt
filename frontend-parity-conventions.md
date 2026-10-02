# Wireshark Cross-Frontend Parity Conventions

This file records durable review conventions for code duplicated or intentionally mirrored across Wireshark frontends. Current upstream source remains authoritative.

## Fix mirrored frontend behavior together when the implementation is copied

When Wireshark and another frontend carry copied or deliberately parallel UI code, a correctness fix in one copy should trigger an audit of the other copy in the same change. Leaving one frontend behind turns a local fix into a predictable divergence.

Merged master MR !9901 works around QTBUG-106718 by making export/import actions use `Qt::QueuedConnection`. Gerald Combs immediately required the corresponding Logray path to be fixed as well, and John Thacker clarified that copied Wireshark/Logray code should receive the same correction in the same commit. The accepted diff updates both frontends. Merged !9912 independently received the same review direction from Gilbert Ramirez and was updated to cover both Wireshark and Logray.

**Review rule:** when a diff changes code with a mirrored frontend implementation, search for the corresponding copy before approval. If the semantics are intended to remain aligned, update both copies and test the affected build paths together; if they intentionally diverge, document why.

**Dependency rule:** a safe workaround for a known framework bug may remain necessary even after an upstream fix exists if Wireshark's supported distribution/dependency floor still includes affected versions. In !9901, Tomasz Moń explicitly noted that Linux distributions would lag the fixed Qt releases and that the queued connection was a low-cost compatibility workaround.

**Confidence:** Very high. Merged master fix with explicit review from Gerald Combs and John Thacker, independently corroborated by the same parity review in merged !9912.
