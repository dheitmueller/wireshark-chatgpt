# Review findings — !1910–!1960, corpus gap !1956

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed the exact 50 MRs in `ledger-1910-1960-gap1956.md`. `mr_1956.json` is absent from this corpus commit. Forty-eight reviewed MRs are merged; closed !1955 and !1934 are down-weighted.

## Strongest findings

- **!1935:** João Valverde removes `HAVE_PLUGINS` from public library headers. No-plugin builds retain stable registration declarations/stubs and runtime capability queries. Guy Harris accepts harmless empty internal lists for simpler code and requires new exports in Debian symbol manifests.
- **!1946:** Pascal Quantin says returning 0 after some Git pkt-lines were parsed wrongly reports protocol rejection. The merged path attaches expert information to the malformed remainder, terminates the loop, and retains the successful prefix. Jonathan Nieder catches the no-progress infinite-loop risk.
- **!1942:** Pascal Quantin rejects a parser helper that both modifies an offset through a pointer and also returns a byte count for the caller to add. Choose one cursor-ownership model and document it.
- **!1948:** TCAP makes child-dissector acceptance explicit and skips fallback decoding after a child handles the component.
- **!1953/!1954/!1958:** Frame Relay dispatch in Linux cooked captures follows `sll.hatype == ARPHRD_FRAD`; Guy Harris notes that the hardware-address type, not the protocol field, is the payload discriminator and asks for a public sample capture.
- **!1955/!1957/!1958:** the release-3.2 backport needed missing ARPHRD definitions. Guy backported the prerequisite first, then the dependent change, and closed the obsolete submission.
- **!1922:** Pascal Quantin steers Git special pkt-lines to one `git.packet_type` field with a `value_string`, and rejects mismatched display/filter terminology and unrelated review churn.
- **!1914:** João Valverde removes generated `config.h` from shared/public-facing headers.
- **!1925:** João Valverde bounds subprocess test logs by keeping head and tail while preserving complete output for assertions.
- **!1960:** Guy Harris documents how WSLua `Proto.init` syntax reaches the global init-routine table.
- **!1944/!1929/!1931:** Guy Harris factors pcapng option processing and gives extension handlers explicit read/write responsibilities. !1931 is qualified by the no-plugins regression later corrected in !1965.
- **!1910:** Guy Harris fixes a pcapng BPF byte-swap copy/paste error so the just-read operand, not the opcode, is swapped.

## Other reviewed work

The remaining merged MRs cover JSON media-type binding, ARPHRD definitions, internal linkage, RTP UI/state, SIP index mapping, O-RAN extension parsing, documentation, time encodings, TCP conversation orientation, Bluetooth ATT reassembly, TECMP/ASTERIX field corrections, and an S7COMM array-parameter cleanup. Closed !1934 was an already-landed/empty GitLab MR and carries no independent implementation weight.

No SMPTE ST 291/VANC packet type was encountered.
