# Wireshark Split-Output Metadata Conventions

When one input is divided into multiple independent output files, each output must carry the descriptive metadata needed to interpret its own records. A later file must not rely on metadata emitted only in an earlier file.

Merged Guy Harris MR !1146 applies this to editcap interface-description metadata; !1147 carries the fix to a release branch. Guy's !1142 separately documents that the writer API makes its own copy of an added interface-description block.

**Review rule:** test later split files independently, including cases where their records refer to metadata first encountered before the split.

**Confidence:** Extremely high; merged master implementation and documentation authored by Guy Harris, with a merged stable backport.
