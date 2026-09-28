# Review findings: Wireshark MRs 6211–6260

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

This inventory records all 50 MRs reviewed in this run. Merged master work is weighted most heavily; stable-branch backports corroborate accepted fixes; closed or superseded MRs are retained only as lower-weight review-history evidence.

| MR | Outcome | Author | Finding |
| --- | --- | --- | --- |
| !6260 | merged | Martin Mathieson | Standardizes repeated --file selection across five checker scripts, including per-file path normalization and explicit missing-file failure. Strong tooling-consistency evidence. |
| !6259 | merged, release-3.4 | Guy Harris | Stable backport of the SLL2 CAN pseudo-header endian fix from !6257. Corroborates that the bounds-checked byte-order correction mattered across maintained branches. |
| !6258 | merged, release-3.6 | Guy Harris | Stable backport of !6257; same bounded, alignment-safe SLL2 CAN post-processing semantics. |
| !6257 | merged | Guy Harris | Adds SLL2 CAN pseudo-header byte swapping only after checking effective packet size and protocol. The temporary struct overlay is never dereferenced; byte-wise swap keeps the operation alignment-safe. Exceptionally strong portability/Wiretap evidence. |
| !6256 | merged | Gerald Combs | Weekly generated-data refresh produced uncompilable ASTERIX C because quoted source text was not escaped. Gerald manually omitted/reverted that generated artifact and the generator was fixed separately in !6262. Strong generated-source workflow evidence. |
| !6255 | merged, release-3.6 | Gerald Combs | Routine weekly registry/documentation refresh; useful corroboration of maintained-branch generated-data synchronization, but little new architectural evidence. |
| !6254 | merged, release-3.4 | Gerald Combs | Routine maintained-branch weekly registry refresh; no distinct durable convention beyond generated-data maintenance. |
| !6253 | merged | João Valverde | Display-filter scanner no longer interpolates non-printable input into a quoted diagnostic; it emits a semantic message instead. Diagnostics must remain valid display text even for invalid lexical input. |
| !6252 | merged, release-3.4 | Uli Heilmeier | Stable backport of the protocol-wiki URL correction. |
| !6251 | merged, release-3.6 | Uli Heilmeier | Stable backport of the protocol-wiki URL correction. |
| !6250 | merged | Uli Heilmeier | Composite TVB append/prepend now treats NULL or zero-length members as a local no-op. Accepted alternative to globally changing subset constructors to return NULL. |
| !6249 | closed | Uli Heilmeier | Proposed returning NULL from zero-length TVB subset constructors, then abandoned after Developer Den discussion because every caller would need a NULL-contract audit and the condition can represent malformed input, missing implementation, or a dissector bug. Negative evidence only. |
| !6248 | merged | Roman Volkov | Adds MPEG TVA ID descriptor decoding; normal protocol-extension work with no notable review discussion. |
| !6247 | merged | Roman Volkov | Corrects a Content Identifier descriptor tag comment; routine source-accuracy maintenance. |
| !6246 | merged | Roman Volkov | Adds MPEG PDC descriptor fields from the specification; routine accepted dissector extension. |
| !6245 | merged | John Thacker | Fixes a bitmask test that used adaptation-field size constants instead of adaptation-field type values. Bit positions must come from the discriminator domain, not an associated size domain. |
| !6244 | merged | John Thacker | Treats 802.11 SSIDs as raw bytes when their encoding is unspecified, presents them with UTF-8-printable byte formatting, and preserves raw bytes for decryption state. Do not invent an ASCII/UTF-8 validation contract the wire format does not guarantee. |
| !6243 | merged, release-3.4 | Uli Heilmeier | Stable backport of PFCP flow-description offset correction. |
| !6242 | merged, release-3.6 | Uli Heilmeier | Stable backport of PFCP flow-description offset correction. |
| !6241 | merged | David Perry | Large root-file formatting/modeline cleanup remained behavior-neutral; Gerald caught a local indentation miss. Formatting migrations should stay mechanical and reviewable. |
| !6240 | merged | David Perry | Replaces direct construction of one long compile/runtime feature string with a structured feature list and deferred formatting. João required mechanism versus formatting-policy separation and preferred direct capability tests over parsing presentation strings. |
| !6239 | merged | diego dupin | Corrects MySQL/MariaDB 0xfd length-encoded integer semantics to three following bytes, with protocol references. Accepted master outcome supersedes !6237. |
| !6238 | merged | John Thacker | Rewrites a search loop to compare pointers directly instead of subtracting them, fixing a 32-bit Windows build issue. Prefer representations that avoid unnecessary pointer-difference width conversions. |
| !6237 | closed | diego dupin | Earlier duplicate of the MySQL/MariaDB length fix that merged as !6239. Superseded; no independent precedent. |
| !6236 | merged | Uli Heilmeier | Master PFCP fix consumes the length field then decodes the value at the current offset rather than adding two a second time. Reinforces disciplined offset ownership. |
| !6235 | merged | Martin Mathieson | Small RLC-NR cleanup while preparing sequence analysis/taps; no major durable convention. |
| !6234 | merged | Chuck Craft | Corrects Decode As model tooltips so each column describes its actual semantic role. |
| !6233 | merged | João Valverde | Adds missing display-filter keyword spellings to the reserved-name registry. Grammar/language keywords must stay synchronized with identifier-registration constraints. |
| !6232 | merged | Alexis La Goutte | Adds Fortinet vendor-specific 802.11 model/serial fields; routine protocol extension. |
| !6231 | merged | Anders Broman | Follow-up to range preferences: separates preference application from handoff initialization and snapshots current TCP/UDP range values after registration. Preference changes need an explicit state-refresh path. |
| !6230 | merged | Martin Mathieson | Improves spelling tooling by recognizing number-plus-unit lexical patterns rather than maintaining a growing dictionary of every numeric spelling. Prefer dependable grammar/pattern rules over exception-list explosion. |
| !6229 | merged | Uli Heilmeier | Master protocol-wiki URL correction; John Thacker notes the destination may evolve, but links must resolve correctly and centralized base URLs ease migration. |
| !6228 | merged | Anders Broman | Changes RTPProxy's configurable TCP/UDP port preference from a single integer to a range; !6231 supplies the preference-state follow-up. |
| !6227 | merged, release-3.6 | Alexis La Goutte | Stable backport of JA3 GREASE exclusion. |
| !6226 | closed | Trond Norbye | Snapshot-marker timestamp proposal exposed version/length ambiguity. Stig Bjørlykke requested that unknown future bytes remain visible; author closed to refactor version-aware decoding. Useful lower-weight extensibility evidence only. |
| !6225 | closed | David Perry | Proof-of-concept for structured About/version information. João endorsed the structured-list mechanism but requested a clean/squashed successor, separation of formatting policy, and avoidance of VCSVERSION-induced libwireshark relinks. Superseded by merged !6240. |
| !6224 | merged | Uli Heilmeier | Master JA3 fix omits TLS GREASE cipher-suite and group values from the canonical fingerprint. Derived fingerprints must implement their specification's canonicalization/exclusion rules. |
| !6223 | merged | Gerald Combs | proto_item_fill_label now rejects a NULL destination explicitly and initializes a non-NULL output buffer to an empty C string before any early return. Strong output-parameter contract evidence. |
| !6222 | merged | Trond Norbye | Adds Couchbase status/command values and aligns VBucket presentation with product logs; routine dissector evolution. |
| !6221 | merged | David Perry | Adds browser-destination tooltips and fixes URL path composition. UI actions that leave the application benefit from exposing the actual destination. |
| !6220 | merged | João Valverde | Tree-wide cleanup removes redundant ENC_NA from ASCII string-item encodings and fixes the checker/fixer accordingly. Plain string fields have character encoding, not integer endianness. |
| !6219 | merged, release-3.4 | Gerald Combs | Stable backport: decode HTML entities in IEEE manufacturer names at generator ingestion. |
| !6218 | merged, release-3.6 | Gerald Combs | Stable backport of manufacturer registry entity normalization. |
| !6217 | merged | Gerald Combs | Help-text updater strips only the volatile extended version fragment with a targeted regex instead of deleting the final whitespace token heuristically. Normalize generated comparison noise precisely. |
| !6216 | merged, release-3.6 | Gerald Combs | Initializes 802.11 local buffers to eliminate Valgrind uninitialized-memory paths. Routine memory-safety correction. |
| !6215 | merged | Gerald Combs | Removes a redundant Windows README and updates installer/build references; merged after later rebase, showing obsolete duplicate documentation should be retired when authoritative docs exist elsewhere. |
| !6214 | merged | Jaap Keuter | CFM encoding/whitespace/comment cleanup; accepted maintenance work with little novel architectural evidence. |
| !6213 | merged, release-3.6 | Gerald Combs | Routine weekly generated registry/help update for a maintained branch. |
| !6212 | merged | Gerald Combs | Master weekly generated update; ASTERIX generated data participates alongside registry/help refreshes. Later !6256 demonstrates that generated output must still pass compilation. |
| !6211 | merged, release-3.4 | Gerald Combs | Routine weekly generated registry/help update for a maintained branch. |

## Evidence weighting

The four closed MRs were not treated as accepted implementation precedent. !6249 and !6225 are useful because their rejected/reshaped designs are directly contrasted with merged successors !6250 and !6240. !6237 is simply superseded by !6239. !6226 contributes only lower-weight extensibility review. Guy Harris's authored !6257 and its two stable backports receive especially high weight for capture-file, byte-order, bounds, and alignment semantics; Gerald Combs's !6223 and generated-update handling in !6256 are also strong project-wide evidence.
