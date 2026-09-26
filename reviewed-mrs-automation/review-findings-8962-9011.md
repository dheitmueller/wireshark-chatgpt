# Wireshark MR review findings: !8962-!9011

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs in descending order: !9011 through !8962. Outcome: **46 merged, 4 closed/unmerged, 0 open**. Merged work is treated as accepted evidence; closed work is retained only for review/process lessons and is down-weighted relative to merged successors.

## Highest-value durable findings

### Heuristic recognition must precede presentation side effects

The strongest finding is !8978, with direct Guy Harris review and stable follow-ups !9009/!9010. A protocol bound to unregistered UDP port 49999 needs structural recognition strong enough not to claim unrelated ephemeral-port traffic. Recognition happens before columns or protocol-tree state are changed. Guy also explicitly required `tvb_reported_length_remaining()` for the declared-frame-length comparison so a sliced capture is not confused with a different protocol. Promoted to `heuristic-dissector-conventions.md`.

### Protocol-tree string values are semantic data, not preformatted labels

!8990 updates the public `proto_tree_add_string()` contract directly. Once packet bytes have been decoded to the protocol's semantic string value, display escaping or formatting should not be baked into that value merely for presentation because display filters and downstream consumers observe it. Promoted to `protocol-text-api-conventions.md`.

### Generic UTF-8 builders should not impose hidden octet ceilings

!8964 removes `wmem_strbuf`'s generic maximum-size parameter. João Valverde cites complexity and UTF-8 boundary problems; Gerald Combs explains the original runaway-append motivation but accepts explicit caller-side truncation as the better policy boundary. Promoted to `string-api-conventions.md`.

### Repeated full-prefix copying makes decompression growth quadratic

John Thacker's !8992 replaces allocate/copy/free growth in `tvb_uncompress()` with `g_realloc()`, specifically to avoid pathological repeated prefix copying at extreme compression ratios. Promoted to `memory-management-conventions.md`.

### Imported algorithm code must pass both provenance and independent tests

!8973 is unusually important despite its old MR number: it remained open for years and finally merged on August 12, 2026. Alexis La Goutte enforced source-license cleanup, Michael Mann insisted failing decompression unit tests be fixed rather than removed, and John Thacker tightened object allocation. The reusable `wsutil` library landed before the dependent SAP dissector integration. Promoted to `contribution-licensing-conventions.md`.

## Strong corroborating and qualified evidence

- !8991 plus !9003 are early provenance for the decompression-progress/resource rules seen more strongly in later reviewed !9014/!9015/!9020/!9021/!9060: drive streaming decompression from actual input consumption, interpret library statuses according to their documented contract, and place an independent bound on attacker-controlled loops/resources.
- !8976/!8986 are early versions of the BPv6 coordinate bug later represented more strongly by !9082: `tvb_reported_length_remaining()` returns a length, not an absolute cursor. Returning it from a cursor-returning helper can move parsing backward and loop forever.
- !8977 with backports !8984/!8985 reinforces the parser-progress rule that a packet-declared structure length must be at least its fixed header size before an offset-driven loop is entered.
- !8993 is useful history for `check_typed_item_calls.py`, but later reviewed !9283 is stronger: this early implementation explicitly says it still needs to tie lookalike arrays to actual bitmask calls; !9283 completed that design.
- !8963 is master provenance for the EAP endpoint-override approach. Later Guy-Harris-authored !9373 remains the higher-authority statement. Review here also required an unrelated Qt compiler workaround to move to separate !9260.
- !9000 independently follows generated-source discipline by changing the E2AP ASN.1/config source and regenerated output together.
- !8995 records Alexis La Goutte steering a new dissector contribution from repetitive switches toward `value_string`/`range_string` tables.
- !8967 records Alexis catching a real field-type/length mismatch in DPoE review; !8966 independently fixes a missing NULL terminator in a bitmask field array.
- !8968 updates the development guide to prefer Visual Studio 2022, but Gerald explicitly required retaining documentation that VS2019 remains supported because the actual CMake baseline still allows it.

## Down-weighted negative evidence

Closed !8998 proposed duplicating CMS/ASN.1/X.509 machinery in a private plugin. Alexis La Goutte asked for a standalone/integrated direction and Anders Broman said it should instead be made more Wireshark-integrated, likely via the existing CMS ASN.1 source. This is strong architecture feedback but not accepted implementation.

Closed !8982 reinforces the established topic-branch submission rule. The contributor's fork master was protected, preventing maintainers from rebasing or making minor edits; Alexis directed creation of a separate branch and a new MR.

Closed !9004 attempted to change core reassembly handling of retransmitted fragments after defragmentation. A fuzz capture then showed new "[Illegal Segments]" regressions in previously valid SMB traffic. The work was abandoned/down-weighted relative to the later narrower fallback API.

Closed !8970 is useful capability-detection evidence: Gerald Combs initially proposed assuming `/Qspectre` from the supported Visual Studio version, then withdrew the change after remembering that the mitigation libraries are an optional installation component.

Merged !8971 is also deliberately not generalized. Testing initially appeared positive, but later John Thacker found severe Qt Wayland problems with the `winId()` approach and pointed to later !11677.

## Other notable merged results

!9008 is a Gerald Combs-authored conversion from old Qt SIGNAL/SLOT strings to typed new-style connections. !8996 is a related explicit-menu-connection refactor, but a post-merge report exposed a missing "Add as Column" action, so it is not used as a clean exemplar.

!8988 and !9001 are substantial TCP sequence-analysis changes. Their validation relies on multiple captures and distinguishes analysis annotations from reassembly behavior; they do not introduce a broader coding convention beyond the notebook's existing state-analysis/testing guidance.

!8979 is a Guy Harris-authored UI fix that makes double-click activation depend on the semantics of the clicked column rather than treating every double click as "start capture".

!8981/!8980 validate `fileno()` before passing the result to GLib, avoiding assertions when a standard stream has no valid descriptor. !8962 similarly makes the display-filter compiler's failure contract total by always returning an error string to the caller, even on an unexpected internal failure path.
