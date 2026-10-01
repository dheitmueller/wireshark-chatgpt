# Dissector registration semantics

## Named and table registration serve different call paths

A dissector does not need a global name merely to be valid. Integer/string dissector-table registration already supports keyed dispatch. Add named registration when another consumer needs to retrieve and invoke that exact dissector directly.

In merged !2322, Guy Harris corrects the premise that DECT was "registered incorrectly" because it lacked `register_dissector()`. The contributor then gives a concrete Lua use case, `Dissector.get("dect")`, and the accepted change adds the name.

**Implementation rule:** choose registration according to the intended dispatch mechanism. Add a global name when direct lookup is a supported extension point, not as ceremonial boilerplate.

**Confidence:** Extremely high. Direct Guy Harris guidance incorporated into a merged master change.
