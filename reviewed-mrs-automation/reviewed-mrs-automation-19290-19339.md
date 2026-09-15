# Automated MR review: !19290-!19339

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`
Direction: descending from newest available unreviewed MR toward older MRs.

This run reviewed exactly the following 50 previously unreviewed merge requests. Selection was made after consulting the existing per-run ledgers and `reviewed-mrs.md`; the previously reviewed !17571-!17620 batch remains part of the already-reviewed set.

!19339, !19338, !19337, !19336, !19335, !19334, !19333, !19332, !19331, !19330,
!19329, !19328, !19327, !19326, !19325, !19324, !19323, !19322, !19321, !19320,
!19319, !19318, !19317, !19316, !19315, !19314, !19313, !19312, !19311, !19310,
!19309, !19308, !19307, !19306, !19305, !19304, !19303, !19302, !19301, !19300,
!19299, !19298, !19297, !19296, !19295, !19294, !19293, !19292, !19291, !19290.

## Durable findings

- !19316 (merged, master; John Thacker): EK is machine-readable output and should request `FTREPR_JSON`, not human-oriented `FTREPR_DISPLAY`. Doing so also removes exporter-local formatting exceptions that belong in the shared ftype representation layer.
- !19329 (merged, master; John Thacker): conversation/endpoints taps gained an explicit persistent machine-readable-output preference shared by CLI/GUI behavior. Guy Harris later emphasized that output optimized for humans and programs are distinct contracts. Added this together with !19316 to `locale-serialization-conventions.md`.
- !19300 (merged, master; Guy Harris): raw IPv4/IPv6 link-layer handling uses a child TVB before handing data to the IP dissector so child changes to packet length do not mutate the top-level TVB. This strongly corroborates existing TVB/subset boundary conventions and was not duplicated.
- !19339 (merged, master; John Thacker): TLS/DTLS decrypted-record bookkeeping stores the record sequence number at successful decryption and exposes it as generated metadata; useful state/presentation evidence but no distinct new convention.
- !19338 was closed unmerged shortly after submission, so it was weighted below merged successors/accepted work.
- !19336 merged after iterative PROFINET MRP-Interconnection review; useful protocol-specific review evidence but no sufficiently general new rule beyond existing value-string/helper-reuse and dissector hygiene guidance.
- !19334 (merged, master; John Thacker) corrects Elastic mapping behavior so omitted string fields remain dynamically indexable. Useful exporter correctness evidence, but !19316/!19329 provide the stronger reusable serialization rule.

The remaining MRs in the exact set above were reviewed as part of the batch and did not justify additional durable notebook rules beyond conventions already represented. Merged MRs were weighted more heavily than closed/abandoned work, and authoritative maintainer evidence was given correspondingly higher weight.