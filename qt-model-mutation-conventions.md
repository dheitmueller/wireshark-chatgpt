# Wireshark Qt Model-Mutation Conventions

## Fix stale views at the owning model, not by synthetic reselection

Closed MR !6433 proposed forcing packet reselection after a packet-comment change so the protocol tree would redraw. Roland Knall rejected that direction: the underlying comment mutation had bypassed the packet-list model, so normal model notifications never reached dependent views. A one-off navigation or repaint path would fix only one symptom.

**Rule:** data owned by a Qt model should be mutated through that model, or through a path that emits the model's authoritative change signals. Do not simulate selection, navigation, or manual redraw to compensate for missing model notifications.

**Evidence weight:** Supporting negative evidence only because !6433 was closed and its code was not merged. The model/view architecture critique is explicit and durable.

## UI visibility is not semantic eligibility

Merged MR !6452 separates hidden-interface presentation from capture eligibility: an interface intentionally shown despite being hidden can still be explicitly selected for capture. Review also exposed an API call newer than the declared Qt baseline, corrected by merged !6456.

**Rule:** view filtering/hiding should not silently change the underlying object's eligibility for an operation, and UI changes must stay within the declared dependency baseline.

**Confidence:** High for the merged visibility/baseline behavior; supporting only for the closed refresh workaround.
