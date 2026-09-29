# Review findings: Wireshark MRs !4961-!5010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are weighted above closed or superseded submissions. Backports are treated as corroboration of the master change unless they add independent review evidence.

| MR | Outcome | Depth | Findings |
|---|---|---|---|
| !5010 | merged | Deep | README.dissector strengthens the naming rule for protocol and field filter names: new names should be lower-case even though compatibility code may continue accepting upper/mixed case. Read with !4968, this is a style/forward-compatibility rule rather than permission to crash on existing uppercase names. |
| !5009 | merged | Deep | Reworks byte-string formatting so the limiting argument is expressed in source bytes rather than output characters, uses zero to mean no explicit limit, renames the API for the semantic break, and adds boundary tests for zero/one/two/full lengths. Strong API-unit and contract evidence. |
| !5008 | merged | Discussion-focused | MKA accepts the two protocol-defined SAK USE lengths, including the zero-length form, and marks other lengths undecoded. Alexis suggested a switch; Jaap preferred consistency with nearby code while there are only two valid lengths. |
| !5007 | merged | Scanned | wslog option parsing checks for a missing option value before passing it to file-opening code. Straightforward defensive argument validation. |
| !5006 | merged | Scanned/backport | release-3.6 backport of BBLog/TCP window-scaling support; master origin is !4987. |
| !5005 | merged | Scanned/backport | release-3.6 backport of the Qt epan cleanup lifetime fix; master origin is !4974. |
| !5004 | merged | Deep | Fixes a format-argument bug in the Skinny generator itself. Reviewer explicitly asked whether regeneration was needed; author confirmed output was unchanged. Generated-source fixes belong in the generator/source-of-truth, with regeneration impact checked explicitly. |
| !5003 | merged | Scanned | asn2wrs method signature is corrected so the generator's own call contract matches the implementation. |
| !5002 | merged | Scanned/backport | release-3.6 UAT compatibility backport of !4983, including defaults for missing trailing fields and tolerance for extra fields. |
| !5001 | merged | Scanned/backport | master-3.2 backport of the forward-compatible UAT behavior that tolerates extra trailing fields. |
| !5000 | merged | Scanned/backport | release-3.4 backport of the forward-compatible UAT behavior that tolerates extra trailing fields. |
| !4999 | merged | Deep/backport | HTTP/2 streaming reassembly maps every contributing DATA frame to the actual multisegment PDU instead of relying on nearest-key lookup; includes a focused gRPC streaming regression capture/test. Corroborates exact historical reassembly-state lookup. |
| !4998 | merged | Scanned | GSM-MAP accepts the enumerated delivery-failure cause form in addition to the structured form; protocol-specific compatibility. |
| !4997 | merged | Deep | FPP reassembly/conversation/proto-data identity is expanded to include capture interface ID and packet direction. Jaap Keuter explicitly rejects mutating `pinfo->p2p_dir`; the accepted code derives a local composite identity instead. |
| !4996 | closed | Discussion-focused | Draft 802.11be work accumulated newer draft changes and was eventually superseded by !7312. Alexis also called out a Clang analyzer dead-store warning. Not accepted implementation precedent. |
| !4995 | merged | Deep | Display-filter semantic checking moves from stringly-typed operator names toward the operator enum and consolidates comparison capability via `ftype_can_cmp`. Errors still render the user-facing operator spelling. Useful internal representation cleanup. |
| !4994 | merged | Scanned | CMake/MSYS2 invokes the wrapper actually executable without a shell (`asciidoctor.bat`) and updates setup documentation. |
| !4993 | merged | Deep | AUTHORS generation stops duplicating hand-maintained header/trailer content inside the script and instead reads the canonical source file. Strong source-of-truth/avoid-duplicated-generated-input evidence. |
| !4992 | merged | Discussion-focused | TCP obvious-retransmission RTO walks prior unacked packets and chooses a plausible captured original using wrap-aware sequence comparison. Author explicitly sought validation on multiple captures before marking ready. |
| !4991 | merged | Scanned | Automatic data/translation update; no durable convention. |
| !4990 | merged | Scanned/backport | release-3.6 automatic data update; no durable convention. |
| !4989 | merged | Scanned/backport | master-3.2 automatic data update; no durable convention. |
| !4988 | merged | Scanned/backport | release-3.4 automatic data update; no durable convention. |
| !4987 | merged | Deep | Master BBLog/TCP window-scaling fix. Jaap Keuter requested the custom-option extraction live in the packet-record case where the metadata is semantically valid. The accepted code then initializes TCP conversation scale state from the record metadata. |
| !4986 | merged | Scanned | Documents the complete IPv6 extension-header set and explicitly records why ESP/HIP are not handled as ordinary extension headers in this dissector path. Useful local rationale, not a broad convention. |
| !4985 | merged | Scanned/backport | release-3.6 authors-file correction; no durable convention. |
| !4984 | merged | Discussion-focused | Early merged 802.11be support. Alexis suggested `proto_tree_add_bitmask()`; Richard Sharpe explained why the variable-position HE control layout makes ordinary fixed masks unsuitable. Standard helpers are preferred when the wire layout matches their model, not mechanically. |
| !4983 | merged | Deep | UAT schema compatibility: newer readers may supply defaults for missing trailing optional fields; older readers should tolerate and warn about extra trailing fields rather than discard/reset the whole table. Gerald documented explicit old/new/extra-field test cases and branch-specific backport scope. |
| !4982 | closed | Discussion-focused/superseded | Specialized Qt-only I/O Graph forward-compatibility design. Gerald closed it in favor of the generic UAT solution in !4983. Strong supersession evidence for fixing compatibility at the shared serialization layer. |
| !4981 | merged | Scanned | SPICE adds the protocol-defined H.265 capability bit. |
| !4980 | closed | Workflow | Same SPICE change as !4981, closed after Alexis asked the contributor not to submit from the fork's master branch. Clean topic branches are part of submission hygiene. |
| !4979 | merged | Scanned | Adds DPoE OAM leaf descriptions; protocol-specific data update. |
| !4978 | merged | Deep | Follow-up hardening for IOAM trace parsing adds stronger node-length/remainder/type validation and expert reporting after the earlier infinite-loop bug. |
| !4977 | merged | Scanned | Moves the GitHub lockdown workflow into the only directory where GitHub executes workflows. |
| !4976 | closed | Workflow/superseded | Duplicate RTPS Group GUID attempt; Alexis directs work to !4967. Not accepted implementation evidence. |
| !4975 | merged | Deep | Immediate fix for an infinite loop introduced by !4962: reject zero node length and impossible remaining-length geometry before entering trace-node iteration. Strong parser-progress evidence. |
| !4974 | merged | Deep | Qt no longer retains an `epan_dissect_t` until widget destruction; it copies the bytes it actually needs and cleans the epan object immediately. This prevents teardown-order crashes and reinforces minimizing borrowed subsystem lifetimes. |
| !4973 | merged | Discussion-focused | OMRON heuristic stops rejecting a gateway-count value that old documentation said should be fixed but real deployed devices use differently. Real captures can invalidate over-strict heuristic assumptions. |
| !4972 | merged | Scanned | Corrects IEEE1905 field semantics from RSSI to RCPI and associated types/ranges. Protocol-semantic correctness only. |
| !4971 | merged | Deep | Display-filter grammar rejects chains such as `a == b matches c`; only ordinary comparison operators may participate in comparison chaining. `matches` and `contains` are not accepted as chain operators. Includes syntax regression tests. |
| !4970 | merged | Deep/backport | Thrift limits nested-type recursion with packet proto-depth state and a configurable default, preventing malicious/excessive nesting from recursing without bound. Also corrects compact container position display. |
| !4969 | closed | Workflow | Profisafe fix itself is plausible, but review requested squashing away a merge commit and moving work off the contributor's master branch. Closed/unmerged, so only workflow evidence is retained. |
| !4968 | merged | Deep | Fixes a crash when protocol/preference names contain uppercase letters by sharing the general valid-name rules; ASN.1 protocol names require compatibility with mixed/upper case. The recommendation remains lower-case for new names. |
| !4967 | merged | Discussion-focused | RTPS Group GUID field type is corrected for v2 while keeping the v1 representation separate. Review also required a proper squashed history and a Wireshark-quality commit message rather than relying on GitLab's merge-time squash option. |
| !4966 | merged | Scanned | Marks an implementation-only SOME/IP helper static. |
| !4965 | merged | Scanned | Corrects duplicate/misattributed author data. |
| !4964 | merged | Deep/backport | DTLS CID handling can fall back to configured client/server CID lengths when handshake packets are absent, and transient AAD storage moves to packet scope instead of fixed maximum stack arrays. |
| !4963 | merged | Deep/high-authority | Guy Harris-authored macOS/CMake fix: only make the Wireshark bundle depend on the `manpages` target when Asciidoctor was actually found and that target exists. Optional feature targets must not become unconditional dependencies. |
| !4962 | merged | Deep/negative-history | Initial IOAM trace support merged with a sample capture but later produced a large/infinite loop. !4975 and !4978 are authoritative for the required progress/bounds checks. |
| !4961 | merged | Discussion-focused | Adds editcap SLL unused-byte normalization for duplicate detection. Chuck Craft required the new CLI option to be documented in the man page and recommended keeping usage text concise while putting full explanation in the manual. |

## High-confidence durable lessons

1. **UAT/configuration schemas need bidirectional version tolerance.** Ignore/warn on unknown trailing fields for forward compatibility and supply defaults for newly added trailing optional fields for backward compatibility. Prefer solving this in the shared UAT layer rather than one GUI feature (!4983; backports !5002/!5001/!5000; supersedes !4982).
2. **Reassembly and conversation identity must include every dimension that separates simultaneous traffic.** For FPP that means interface identity plus direction; derive a local composite key and do not mutate `pinfo->p2p_dir` to smuggle additional identity (!4997, Jaap Keuter).
3. **Loop-entry validation is part of parser correctness.** A zero element width or impossible remaining-length geometry must terminate before iteration; later expert diagnostics should explain the malformed data (!4962 -> !4975 -> !4978).
4. **API limits should be expressed in the semantic unit callers naturally possess.** !5009 changes the byte formatter's limit from output characters to source bytes, names the API accordingly, and tests all truncation boundaries.
5. **Only genuinely chainable display-filter operators belong in comparison chains.** Grammar should reject `matches` and `contains` in chained comparisons rather than accepting them and hoping semantic checking recovers (!4971).
6. **Optional build artifacts cannot be unconditional target dependencies.** Guard dependencies on generated targets with the capability/tool condition that causes the target to exist (!4963, Guy Harris).
7. **New filter/protocol field names should be lower-case, while parsers/registration code remain compatible with historical uppercase names.** Style and compatibility are distinct concerns (!4968 + !5010).
8. **Generated outputs need one authoritative input.** Fix generator code and canonical source material rather than maintaining duplicated generated-input text in scripts or hand-patching generated products (!5004, !4993).
