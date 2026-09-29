# Conventions from Wireshark MRs !4961-!5010

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

## UAT schema compatibility

Merged !4983 establishes a shared compatibility model for persisted UAT rows. Older readers accept unknown trailing fields with a warning instead of discarding the table, while newer readers can supply defaults for newly added trailing optional fields. Gerald Combs documented old-row, new-row, and extra-field cases. !5002, !5001, and !5000 are stable-branch corroboration; closed !4982 was superseded by this shared-layer solution.

**Rule:** when appending fields to persisted configuration rows, make old readers tolerate extra trailing data and make new readers define explicit defaults for new optional trailing fields. Test both directions and write-back behavior.

## Stateful link-layer identity

Merged !4997 fixes FPP state collisions by including capture interface ID plus packet direction in conversation, proto-data, and fragment-table keys. Jaap Keuter explicitly required keeping `pinfo->p2p_dir` unchanged and deriving a local composite identity instead.

**Rule:** state/reassembly keys must include every dimension that separates concurrent traffic. Packet metadata describing the observed packet is input state, not scratch storage for a private key.

## Parser loop progress

Initial merged IOAM work !4962 was followed by !4975 after a non-terminating malformed-input case. The fix rejects zero node length and impossible remaining-length geometry before iteration; !4978 adds stronger checks and expert reporting.

**Rule:** validate a packet-derived loop stride/count before entering the loop. Reject zero progress and values that cannot fit inside the enclosing object.

## Semantic API units

Merged !5009 changes byte-string formatting limits from output-character count to source-byte count, names the parameter accordingly, uses zero for unlimited display, renames the API for the contract change, and adds boundary tests.

**Rule:** express limits in the semantic unit callers naturally possess. If the unit/meaning changes, make the API break explicit rather than preserving a misleading name.

## Display-filter chaining

Merged !4971 makes constructs such as `a == b matches c` invalid syntax and also disallows chaining `contains`; only ordinary comparison operators participate in comparison chains.

**Rule:** chainability is an explicit property of operators and belongs in the grammar/operator model. Add positive and negative grammar tests when changing this area.

## Optional CMake targets

Guy Harris-authored merged !4963 fixes macOS configuration without Asciidoctor by adding the Wireshark-to-`manpages` dependency only when Asciidoctor is found and the target exists.

**Rule:** if an optional capability controls target creation, guard every downstream dependency/reference with the same capability condition.

## Filter-name style versus compatibility

Merged !4968 keeps registration compatible with existing mixed/upper-case protocol names, including ASN.1 names. Later merged !5010 strengthens the style recommendation that new protocol and field filter names use lower case.

**Rule:** use lower-case names for new work, while treating compatibility with already-valid historical names as a separate concern.

## Generated-source ownership

Merged !5004 fixes the Skinny generator itself and explicitly checks whether regeneration changes output. Merged !4993 removes duplicated AUTHORS boilerplate from the generator and reads canonical source data instead.

**Rule:** fix the generator or canonical source-of-truth, not only generated products, and avoid duplicate hand-maintained copies of generator input.

## Submission history

!4980 and !4969 contain direct review requests to work on a topic branch rather than the contributor's master branch. !4967 also shows that a merge-time squash checkbox is not a substitute for preparing a clean, well-messaged reviewable series.

**Rule:** submit from a topic branch, remove accidental merge/noise commits, and make the branch history and commit messages reviewable before merge.

## CLI documentation

In merged !4961, Chuck Craft required the new editcap option to be documented in the man page and suggested keeping the synopsis terse while moving detail into the reference documentation.

**Rule:** new user-visible command-line options require matching tool documentation.
