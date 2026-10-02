# Key findings: 1410-1459

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

The exact 50-MR accounting is in ledger-1410-1459.md. This file records the strongest reusable findings.

- 1454 and 1457: Guy Harris-authored master changes replace toggle-style editcap flags with one-way Boolean state and reject duplicates. Stable backports 1455, 1456, 1458, and 1459 corroborate the contract.
- 1451: MPTCP v1 changes the handshake direction that establishes key/state ownership. Later MP_JOIN state mapping must therefore use the negotiated version. A supplied capture exercises negotiation, an added subflow, and data.
- 1450: TCP window analysis must respect the transition from handshake semantics to negotiated steady-state semantics.
- 1448: repeated Bluetooth LE control-PDU layouts are factored into helpers before richer stateful tracking; review also requested a cleaner squashed history.
- 1433: expensive Preferences Advanced search work is debounced with a restartable single-shot timer so intermediate keystrokes do not trigger full updates.
- 1425 with 1434: shallow Git history is appropriate only for CI jobs whose semantics do not need full repository history; version tooling should also handle incomplete CI history deliberately.
- 1418: Pascal Quantin caught an iLBC change that broke the older dependency still used on Windows and directed use of the dependency's own version signal.
- 1414: deprecated display-filter syntax can still be valid; deprecation and compile validity are separate states.
- 1413 with 1431, 1438, and 1443: prefer typed GLib/wmem element-count allocators and use project tooling to identify raw allocation patterns.

Closed/unmerged and down-weighted: 1417, 1416, 1415. All other MRs in the batch were merged.

No SMPTE ST 291/VANC packet type was encountered.
