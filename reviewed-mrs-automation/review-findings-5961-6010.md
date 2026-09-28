# Review findings: !5961–!6010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This batch contains 47 merged MRs and three closed/unmerged MRs (!5986, !5970, !5968). Merged work is weighted more heavily; closed work is retained only where it gives useful maintainer review, supersession, or rejected-design evidence.

| MR | Outcome | Review finding |
|---|---|---|
| !6010 | merged | Gerald Combs documentation cleanup; AsciiDoc indentation is made explicit in EditorConfig. |
| !6009 | merged | QUIC v2 packet-type interpretation becomes version-dependent; example pcaps accompany the change and review refines the default/version branch. |
| !6008 | merged | LLRP malformed-short input uses an ordinary bounds guard instead of `DISSECTOR_ASSERT`. |
| !6007 | merged | John Thacker updates manually registered X.509 GeneralizedTime fields to `FT_ABSOLUTE_TIME`; generated-format changes do not automatically fix manual registrations. |
| !6006 | merged | **Promoted.** Martin Mathieson improves typed-item checker discovery by distinguishing registered fields, local declarations, and externally looked-up IDs; checker scope also expands to `file-*.c`. |
| !6005 | merged | PROFINET TSN review repeatedly catches mismatches among `hf_` type, access length, and encoding; checker warnings must be resolved semantically rather than silenced mechanically. |
| !6004 | merged | **Strong Guy Harris evidence.** New Realtek dissector modernizes old code; review catches obsolete build guards and typed-item/encoding mistakes, and Guy corrects the field-type interpretation before fixing the appropriate calls. |
| !6003 | merged | John Thacker GTP' Release 15+ work; Jaap Keuter catches an offset advance and unclear state naming, and asks that a separable bug be kept in a separate change. |
| !6002 | merged | Legal/short 5co values are guarded before calling a helper whose contract requires a positive byte count. |
| !6001 | merged | Kerberos manual GeneralizedTime registration follows the same `FT_ABSOLUTE_TIME` semantics as generated BER output, with template/generated output kept in sync. |
| !6000 | merged | **Promoted.** John Thacker explains that per-frame proto data, conversation data, and capture-wide/global state have different identities and lifetimes. A global is wrong when independent protocol relationships can interleave; use the relationship the protocol itself uses, possibly a custom conversation endpoint or internal keyed structure. |
| !5999 | merged | NSIS uninstall uses recursive removal for a directory tree rather than deleting only one level. |
| !5998 | merged | John Thacker adds GTP MBMS UE/Enhanced NSAPI decoding; straightforward accepted protocol coverage. |
| !5997 | merged | **Promoted.** Custom dissector-table registration gains a key-destroy callback, fixing leaks and making caller-defined key ownership explicit. Complex COSE keys unref contained `GVariant` values before freeing the key. |
| !5996 | merged | DHCPv6 avoids calling `tvb_bytes_to_str` with a zero-length value. |
| !5995 | merged release-3.4 | Stable backport of corrected GTP PDP-organization value-string mapping. |
| !5994 | merged release-3.6 | Stable backport of corrected GTP PDP-organization value-string mapping. |
| !5993 | merged | John Thacker corrects GTP PDP organization to use the organization table rather than the PDP-type table. |
| !5992 | merged release-3.4 | Automatic registry/translation/data refresh; no new durable convention. |
| !5991 | merged release-3.6 | Automatic registry/translation/data refresh; no new durable convention. |
| !5990 | merged | Automatic master registry/translation/data refresh; no new durable convention. |
| !5989 | merged | JSON dumper is generalized from FILE output to FILE and/or `GString`; review favors consistent parameter conventions and packet-scope allocation for packet-lifetime results. |
| !5988 | merged | John Thacker adds GTP Additional Trace Information decoding; accepted protocol extension. |
| !5987 | merged | John Thacker catches two source/destination copy-and-paste errors in SOME/IP statistics name resolution. |
| !5986 | closed | Duplicate CDP zero-length assertion fix; Alexis La Goutte points to existing work. Down-weighted as duplicate/superseded. |
| !5985 | merged | Gerald Combs distinguishes broadly shared tool-generated files from personal local artifacts; the latter belong in personal excludes. |
| !5984 | merged | MPEG Mosaic descriptor addition; ordinary style/structure review. |
| !5983 | merged | IPP guards zero-length values before byte-string conversion. |
| !5982 | merged | John Thacker reuses an existing BSSGP decoder from GTP by exposing the helper rather than duplicating parsing. |
| !5981 | merged | MPEG Time Shifted Service descriptor; review asks for expert diagnostics when fixed structural length is violated. |
| !5980 | merged | MPEG Country Availability descriptor; review discusses using appropriate Boolean presentation. |
| !5979 | merged | GTP adds newer IEs previously marked future-use; straightforward accepted coverage. |
| !5978 | merged | **Promoted.** John Thacker anchors checker file matching at `.c$` so editor swap/backup files such as `.c.swp` and `.c~` are not parsed as source. |
| !5977 | merged | John Thacker replaces more undecoded GTP IEs with shared/real decoders. |
| !5976 | merged release-3.6 | Stable backport of the no-ZLib build correction from !5975. |
| !5975 | merged | **Promoted, strong Guy Harris evidence.** A build without ZLib must compile out ZLib implementation paths, reject unavailable CLI gzip selection with accurate supported-value diagnostics, and disable the GUI control. Optional capability must disappear coherently across internals and user interfaces. |
| !5974 | merged | PPP represents a legal zero-length byte string without calling a helper that rejects zero length. |
| !5973 | merged release-3.6 | Stable backport of GTP RAC field-width correction. |
| !5972 | merged | John Thacker adds GTP CSG-related IEs; straightforward accepted protocol work. |
| !5971 | merged | John Thacker keeps the semantic RAC field at one octet and excludes the separate mandated padding octet; field type/access width follow the semantic field, not enclosing layout width. |
| !5970 | closed | Enabled-Protocols Save-button experiment closed after Stig Bjørlykke/Pascal Quantin discussion; do not propagate a controversial persistence UI pattern merely for cross-dialog consistency. |
| !5969 | merged | John Thacker adds GTP Cell Identification IE support. |
| !5968 | closed | **Useful negative evidence.** Anders Broman rejects the proposed H.248 SDP change, supplies a comparison capture, and states that a valid ASN.1-generated fix belongs in the template followed by regeneration. |
| !5967 | merged | ASTERIX generator/spec cleanup and tests change generated presentation in lockstep. |
| !5966 | merged | **Promoted.** Roland Knall cautions that heuristic UI names are protocol-facing/documentation semantics; registration under a transport does not automatically make "X over Y" the correct protocol name. The most contentious renames were reverted. |
| !5965 | merged | **Promoted.** Jaap Keuter and John Thacker resolve an apparent 3GPP table typo by triangulating the bit diagram, value domain, neighboring IE definitions, wording, and a sample capture instead of following one contradictory offset cell literally. |
| !5964 | merged | Protocol enable/disable work adds CLI tests and exposes the difference between temporary session changes and persisted profile configuration; placement/persistence semantics are part of UI design. |
| !5963 | merged | GTP CAMEL container support edits ASN.1 configuration/source and regenerated output, then reuses the generated PDU entry point. |
| !5962 | merged | Martin Mathieson improves checker matching while explicitly documenting remaining false-positive limitations; static source checkers should narrow recognition deliberately. |
| !5961 | merged | clangd cache/compile database ignores are accepted because they are shared artifacts across many editors; one-off personal artifacts remain personal-ignore material. |

## Durable conclusions promoted

1. Persistent dissector state must be keyed by the actual protocol relationship that survives interleaving; do not substitute a capture-wide global or an unrelated transport conversation (!6000).
2. Generic containers with caller-defined key types must make key destruction/ownership an explicit part of their API (!5997).
3. Feature-disabled builds must remove unavailable functionality coherently from implementation, CLI validation/help, and GUI affordances (!5975; stable !5976).
4. Checker source discovery should model the actual source-file grammar and distinguish declarations/definitions/external lookups instead of matching filename/name substrings loosely (!6006, !5978, corroborated by !5962).
5. When a standard contradicts itself, triangulate independent normative structure, value domains, analogous fields, and representative captures before deciding which interpretation is plausible (!5965).
6. Heuristic dissector display names are user-facing semantic labels; transport registration context alone is not sufficient grounds for a convenience rename (!5966).
7. Closed !5968 strongly corroborates the existing rule that generated dissector changes belong in generator/template/configuration inputs and must be regenerated, not patched only in generated C.
