# Wireshark conventions from MRs !1260-!1309

These rules supplement the topical notebook files. Current upstream source remains authoritative.

## Configuration values that control parser progress must be validated

Merged master MR !1285, authored by Martin Mathieson, validates the configured O-RAN sample bit width before deriving the section stride. A zero width is reported with Expert Info and parsing stops because a zero stride would prevent loop progress.

**Rule:** validate preference/UAT/configuration values before they become parser strides, divisors, or element sizes. Accepted values must guarantee deterministic progress.

## Preserve semantic field type independently of display style

Merged master MR !1292, authored by Guy Harris, changes the FC dNS Owner Id from `FT_STRING` to `FT_BYTES` with dotted-byte display because the value is a three-octet FC address, not text. Guy's stable backports !1302 and !1303 carry the same correction.

**Rule:** choose `hf_` types from the protocol value's semantic domain. A binary identifier or partial address does not become text merely because its preferred rendering is compact or dotted.

## Keep issue identity out of the subject's job

Closed !1293 began with the generic subject “Fix the bug 16446.” Guy Harris required the subject to say what changed rather than merely naming the bug. In closed successor !1297, Graham Bloice supplied a component-specific subject/body form and Pascal Quantin clarified GitLab's closing-keyword syntax. The implementation then merged as !1300.

**Rule:** make the subject identify the component and behavior changed; put the issue-closing reference in the message/body. Closed iterations are workflow evidence, while merged !1300 is the accepted implementation.

## Static checker models must cover the real field domain

Merged MR !1273, authored by Martin Mathieson, adds 48- and 56-bit integer widths to the typed-item checker and records a narrowly justified non-contiguous-field exception with a protocol reference.

**Rule:** extend checker models when valid APIs/types outgrow them. Keep exceptions narrow, named, and justified instead of weakening the checker globally.

## Lift size limits end-to-end, not only at the top level

Merged !1275 replaces the 3GPP nettrace reader's whole-file size ceiling with rolling buffering and wide offsets. Guy Harris identified several narrowing warnings in review. The accepted code preserves wide lengths and, where a GLib API accepts only `guint`, processes ranges in `G_MAXUINT` chunks before the final safe cast.

**Rule:** when removing a file-size limit, audit every offset/length representation and API boundary. Keep values wide until a narrower callee is proven safe, or split the operation into bounded chunks. Treat conversion warnings as evidence of a possible hidden size ceiling.

## Use semantic sentinels rather than magic zero for context

Merged !1276 replaces repeated `fc_data.ethertype = 0` initializations with the named `ETHERTYPE_UNK` value. Guy Harris reviewed the issue split and authored stable backports !1282 and !1283.

**Rule:** when a context API defines an explicit unknown/unset value, use it consistently rather than relying on a coincidental integer zero.

## Prefer canonical bitmask and TFS APIs for packed flags

During review of merged !1301, Alexis La Goutte directed the PROFINET RSI implementation toward shared `true_false_string` values and `proto_tree_add_bitmask()`, and the contributor also corrected filter naming and labels to match the specification.

**Rule:** use Wireshark's bitmask/TFS facilities for packed flag groups where they express the wire structure directly; keep field names and labels aligned with the protocol specification.

**Confidence:** High to extremely high. The strongest rules above come from merged maintainer-authored changes or substantive maintainer review; closed MRs are used only for submission-workflow evidence.
