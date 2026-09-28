# Durable conventions from MRs !6611-!6660

Corpus: `ddcaa22b51c68f594e425a23388c3a2086813054`

- **Frames vs PDUs (!6650):** statistics must not overload "packets" when one frame can contain repeated protocol instances. Track frame presence and PDU count separately.
- **Contextual dfilter typing (!6631):** if an operand supplies an expected type, try the ambiguous token as that typed literal/value-string before resolving it as a registered field.
- **Single lexical normalization (!6628):** strip literal syntax markers once in the scanner; avoid retrying converted/original spellings with duplicated error ownership. !6622 and !6614 reinforce edge-case tests.
- **Grammar changes are end-to-end (!6633):** when delimiters already have other syntax roles, choose an unambiguous grouping form and update grammar, semantic checks, docs, release notes, and tests together.
- **Unreassembled is not malformed (!6621, !6623):** mark partial TVBs as fragments when desegmentation cannot occur and report FragmentBoundsError as low-severity reassembly guidance.
- **Encoded width controls cursor movement (!6629, !6644, !6645):** semantic prefix significance does not shorten PREF64's fixed 96-bit wire field.
- **Payload length vs header length (!6626, !6638):** decode the declared payload bytes and advance by length-field header plus payload.
- **Semantic-domain registries (!6646):** packet and log conversation filters use separate registries/lookup APIs so invalid cross-domain choices are not exposed.
- **Submission history (!6642):** Gerald Combs states the project preference to squash an MR into one commit unless distinct commits are meaningfully justified.
- **Lower-weight closed work:** !6619's generalized filtering argument is corroborated by merged !6694; !6620 and !6611 remain review discussion, not accepted implementation precedent.
