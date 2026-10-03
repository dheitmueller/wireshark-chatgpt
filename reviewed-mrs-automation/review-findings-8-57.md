# Review findings — MRs 8 through 57

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 MRs. Outcomes: 31 merged, 18 closed/unmerged, and one open snapshot (MR 13). Closed and superseded proposals were down-weighted as implementation evidence.

## Strong findings

- MRs 26 and 53: Decode As changes must apply affected protocol preferences before redissection. John Thacker documented a TFTP case where derived state otherwise retained a reference to replaced preference data.
- MR 54: Anders Broman rejected silently treating a malformed ASN.1 global opcode as a local opcode. Preserve the standards-defined interpretation and expose protocol violations rather than rewriting malformed traffic into a valid-looking form. Change ASN.1 source/templates rather than generated output alone.
- MR 32: Pascal Quantin rejected a global MBIM version variable because multiple devices can coexist in one capture. Per-flow mode/version belongs in conversation state. He also required an oversized change to be split so the normal diff UI could review it.
- MR 51: Pascal required exact byte highlighting, field types that match the data, encoding justified by the specification or capture, and normal registered-field APIs instead of custom formatting that hides values from non-GUI consumers.
- MRs 24 and 52: Keep wire width, cursor advancement, field metadata, and parser-consumed values aligned. Prefer one registered-field decode when the same value is both displayed and used by parser state.
- MRs 48 and 47: Generated dissectors should be changed through their authoritative ASN.1/template/config sources, with generated output updated in the same change.
- MRs 22, 20, 23, and 18: CI must provide useful signal for Wireshark's actual codebase. Reuse existing jobs, publish useful reports, keep noisy analysis non-blocking, and do not let a checker create project policy before maintainers agree on the policy.
- MR 14: New user-visible filtering semantics should receive release-note and User's Guide coverage. Gerald Combs also required GitLab issue-reference conventions instead of obsolete Gerrit metadata.
- MRs 40 and 36: Attach captures to the issue or MR rather than committing patch/capture bundles into the source tree. Keep unrelated changes out and present a coherent review history.
- MR 57: This early spelling cleanup changed some display-filter identifiers. Later notebook evidence about preserving compatibility-facing identifiers is stronger and should govern current reviews.

MR 13 remained open in the corpus snapshot, so its implementation is provisional. No SMPTE ST 291 / VANC packet type was encountered.
