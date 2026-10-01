# Protocol Tree API Conventions

## Use supported structured subtree APIs for structural grouping

During merged master MR !2831, Anders Broman explicitly stated that the older/general tree method being used should not be used in dissectors and requested `proto_tree_add_subtree()` or `proto_tree_add_subtree_format()` instead. The accepted RTPS Type Object fix uses those APIs, including a formatted subtree for the count of undisplayed elements.

**Rule:** use dedicated subtree APIs for structural grouping nodes. Prefer typed and structured protocol-tree APIs over ad-hoc textual items when Wireshark provides an API that directly models the tree operation.

**Confidence:** very high. Direct Anders Broman review incorporated before merge.
