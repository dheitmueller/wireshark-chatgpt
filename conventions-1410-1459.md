# Conventions from the 1410-1459 MR review

Current upstream source remains authoritative.

## CLI flags

Merged master changes 1454 and 1457, authored by Guy Harris, establish that an enable-style option should set Boolean state rather than toggle it. A duplicate occurrence should be idempotent or rejected; it should not silently reverse the first occurrence. Stable backports 1455, 1456, 1458, and 1459 carry the same behavior.

## Typed allocation

Merged 1413 and follow-ups 1431, 1438, and 1443 prefer GLib and wmem typed allocation macros that take an element count. This makes the element type explicit, avoids manual byte-size multiplication, and reduces overflow-prone allocation patterns. The project also used tooling to identify these patterns.

## Dependency compatibility

Merged 1418 demonstrates that dependency API migrations must account for the oldest versions still present in the supported platform matrix. Pascal Quantin identified an older libiLBC version on Windows and directed the change to use the dependency's own version indicator for the API boundary.

## Display-filter validity

Merged 1414 separates filter validity from deprecation. If a display filter compiles successfully, a caller asking whether it is usable should not reject it solely because deprecated tokens are present. Deprecation should be reported separately from validity.

## Version-dependent protocol state

Merged 1451 shows that a protocol-version change can alter which endpoint owns persistent connection state. MPTCP v1 moves the first-key exchange to the opposite handshake direction from v0, so later subflow mapping must use the negotiated version rather than assuming the original forward-direction model. The change was validated with a capture containing negotiation, an added subflow, and data in both directions.

## Qt event handling

Merged 1433 is a debounce example: expensive text-driven UI work should retain the latest input and run after a restartable single-shot timer expires when intermediate values have no durable meaning. This is distinct from throttling continuous work or merely queueing a repaint.

## Additional corroboration

Merged 1450 reinforces that negotiated transport behavior may have a handshake transition boundary and should not be applied too early. Merged 1448 factors repeated Bluetooth control-PDU parsing before adding richer stateful behavior. Merged 1425 scopes shallow Git history to CI jobs that do not need packaging/version history, while 1434 makes version extraction robust to incomplete CI history. Merged 1429 shows that option-level semantics can make otherwise similar pure ACKs analytically distinct.
