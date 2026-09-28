# Wireshark Qt Model Iteration-Domain Conventions

## Iterate the source model when an operation applies to hidden as well as visible rows

Merged master !6580, followed by Guy Harris-authored/maintained-branch work !6594 and !6595, fixes interface statistics updates when some interfaces are hidden. The old loop used the proxy model's row count, so filtered-out interfaces were skipped. The accepted code iterates `source_model_.rowCount()` and then maps the source index through the proxy/info models as needed.

This complements the existing view-relative rule: the choice of source versus proxy model depends on the semantic set the operation is supposed to cover.

**Implementation rule:** use the source model as the iteration domain when refreshing or mutating all underlying entities, including filtered or hidden ones. Use the proxy model as the iteration domain only when the operation is intentionally defined by the current visible set or order. Mapping through a proxy does not by itself make the proxy's row count semantically correct.

**Confidence:** Very high. Merged master correctness fix plus Guy Harris-authored corroborating branch work.
