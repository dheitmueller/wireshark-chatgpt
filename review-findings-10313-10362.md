# Wireshark MR review findings — !10362 through !10313

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Reviewed 50 previously unreviewed MRs. The strongest recurring lesson is that generated dissector output must be corrected at its authoritative ASN.1 or template source and regenerated. Guy Harris explicitly reinforced this in !10343, !10341, and !10330, with follow-up source corrections in !10352, !10351, and !10353.

Other durable findings are promoted into the focused notebook convention files changed by this run.
