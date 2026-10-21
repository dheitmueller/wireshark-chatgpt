# Conventions from Wireshark MRs !910-!959

## Bitfield metadata

Merged !959 shows that ordinary masked integer and boolean fields should carry truthful type widths and masks so generic proto-tree consumers can derive their bit offset and width. Manual bit-position annotations are for layouts the normal field contract cannot express. The packet diagram should be checked with its debug diagnostics after metadata changes.

Merged !942 adds an adjacent-duplicate-mask check to the typed-item checker. Consecutive component fields with different labels but the same numeric mask are suspicious; mask comparison should use numeric meaning rather than spelling.

## Memory ownership

Merged !938 shows that GLib container destroy callbacks must match the allocator that owns the contained objects. A hash table containing wmem packet-scope keys and values must not call `g_free` on those elements.

## Socket address API contracts

Guy Harris's merged !929 and stable backports !931/!932 distinguish generic storage capacity from the address length required by socket syscalls. Code passing a generic `sockaddr` pointer plus a length should use the concrete IPv4/IPv6 object length, or the platform length member when available. Separating `socket()` and `connect()` failures also preserves useful diagnostics.

## Submission wording

Graham Bloice's review on merged !927 reinforces that a commit subject should describe the resulting component change, not the submission/review history or a previous MR.

## Shared field presentation

Anders Broman's review on merged !950 favors shared `true_false_string` definitions when their semantics already match the field rather than adding local wording variants.

## Parser portability

Merged !955 moves the Protobuf grammar from Bison to Lemon to avoid an unnecessary Bison-specific build requirement. Parser-generator migrations should preserve behavior with representative regression cases and should account for the portability/toolchain contract of the project.

## Static-analysis cleanup

In merged !933, Guy Harris recommended narrowing a variable to the lexical block where it is initialized and consumed. This expresses the control-flow invariant more clearly than widening the variable's lifetime and adding a defensive initialization merely to satisfy static analysis.
