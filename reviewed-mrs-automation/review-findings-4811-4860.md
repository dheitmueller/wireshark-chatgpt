# Wireshark MR review findings — 4811–4860

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly the 50 MRs recorded in the companion exact ledger. Outcome: 49 merged and 1 closed/unmerged (4860). Closed, reverted, superseded, and later-regressed work is down-weighted.

## Strong findings

- **4851** — C12.22 carries logical element length separately from the number of bytes actually allocated and validates both sides before copying.
- **4841** — IDMP copies packet-lifetime protocol identity before retaining it in longer-lived state; the source notes that session-local state would be preferable.
- **4832** — Display-filter range syntax is parsed and validated by a dedicated drange constructor, reducing scanner/parser state and establishing range-node invariants at construction.
- **4825** — README.developer explicitly distinguishes project-internal shared-library APIs from the external plugin API/ABI and documents different compatibility expectations.
- **4820** — text2pcap uses Wireshark's shared ISO-8601 parser rather than depending on platform-specific `strptime()` behavior.
- **4818** — CBOR sequence parsing terminates repetition when the child parser reports structural failure; its fixture generator also gains a raw-input mode.
- **4817** — John Thacker gives BSSAP, BSSAP-LE, and BSAP distinct Decode-As identities while sharing implementation and carrying selected variant state per packet.
- **4816** — John Thacker leaves the BT-DHT heuristic disabled by default until the parser is sufficiently hardened for broad automatic invocation, despite considering recognition itself strong.
- **4857** — Jörg Mayer catches a direct change to generated Skinny output and requires the durable change through the generator/template path; later 4906 is the synchronization fix.
- **4853** — A merged RTP recurrence change was later shown by John Thacker to rely on a predicate whose meaning did not fit the new previous-packet reference point; this is regression history rather than current precedent.
- **4815 / 4823** — Earlier BT-DHT error handling returned a positive remaining-length value; later merged 5280 is authoritative that the caller required the failure sentinel instead.
- **4839** — PVFS threads `packet_info *` through helper layers rather than passing a null context when lower-level parsing needs packet-aware behavior.
- **4833** — BLF maps format-specific receive/transmit direction into standard packet direction metadata across several record types.
- **4846 / 4845 / 4844** — BT SDP validates and caps retained continuation-state length using the protocol limit and reports the invalid condition.
- **4828 / 4829 / 4830 / 4831** and **4842 / 4843** — parser loops are corrected so each iteration advances or terminates.

## Lower-weight and corroborative items

4860 is closed/unmerged with no review discussion and is not implementation precedent. 4859, 4858, 4856, 4855, 4854, 4852, 4850, 4849, 4848, 4847, 4840, 4838, 4837, 4836, 4835, 4834, 4827, 4826, 4824, 4822, 4821, 4819, 4814, 4813, 4812, and 4811 were reviewed and are primarily corroborative, backport, documentation, or release-history evidence.

## Reviewer authority

No substantive Guy Harris technical review appears in this batch. Jörg Mayer's generated-source review and John Thacker's design/diagnostic evidence are given appropriate weight.
