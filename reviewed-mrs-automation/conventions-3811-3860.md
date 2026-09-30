# Durable conventions from Wireshark MRs 3811-3860

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Promoted

1. **Transform capture metadata coherently (!3858, !3860).** If a tool changes link-layer interpretation, update IDBs as well as file/packet encapsulation. Copy retained metadata before transformation when later output files still need the original.
2. **Respect proto-data ownership boundaries (!3855).** Native dissectors should use p_add_proto_data()/p_get_proto_data() with an allocator matching the stored value's lifetime; do not appropriate pinfo->private_table from the Lua API.
3. **Keep machine output canonical and structurally parseable (!3853, !3854).** Emit registry/API machine names rather than human descriptions, and represent repeated per-section data with an explicit schema rather than ad hoc headings.
4. **Check equivalent proto-tree API families consistently (!3848).** Field width/type rules apply to ptvcursor helpers just as they do to direct proto-tree add calls.
5. **Use dissector-specific prefixes for local helpers (!3845).** Reserve proto_-style names for common infrastructure. Avoid gratuitous inline.
6. **Translate offset coordinate systems explicitly (!3841).** A child working on a subset TVB can return a subset-relative offset; the parent must add the subset base before indexing the original TVB.
7. **Include uniqueness context in identity keys (!3811).** If an ID is only bus/interface-local, key mappings by the composite context; make any wildcard fallback explicit.

## Submission and review corroboration

MR !3845 records Pascal Quantin's warning that GitLab's squash option can be lost when the Wireshark utility rebases for linear history. When reviewers ask for a squashed topic, perform the squash locally and leave a meaningful commit message. This corroborates the existing submission-squash rule.

## Superseded or lower-weight evidence

- Closed !3859 is superseded by merged stable backport !3860.
- Closed !3833 is superseded by merged !3837.
- !3832's caller-side wtap_rec_reset() model was later improved by the centrally-owned record-block lifecycle in !4042.
- !3827/!3829 are earlier instances of checker target/platform handling; Guy Harris's later !4273 remains the higher-authority implementation precedent.
