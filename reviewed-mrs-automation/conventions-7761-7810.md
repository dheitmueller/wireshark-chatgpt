# Conventions extracted from Wireshark !7761-!7810

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

## Timestamp range

Guy Harris's merged !7798/!7794/!7795/!7796/!7792/!7789 series keeps timestamps in an appropriate internal time type and validates the narrower destination capture-file representation before conversion.

**Rule:** validate width and signedness at the serialization boundary instead of narrowing internal timestamp state early.

## Child resource inheritance

Merged master !7763 and release-4.0 backport !7785 replace broad Windows handle inheritance with an explicit set of handles intended for the child.

**Rule:** make inherited resources an explicit launch contract. Unrelated inherited duplicates can extend object and pipe lifetimes.

