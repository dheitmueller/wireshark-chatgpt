# Automated MR review batch: !17059–!17108

Corpus commit: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Direction: descending from newest available unreviewed MRs toward older MRs.

Previously reviewed tracking was consulted before selection, including `reviewed-mrs.md` and the per-run files in `reviewed-mrs-automation/`. The historical !17571–!17620 batch remains preserved and counted.

## Exact reviewed MR set

!17108, !17107, !17106, !17105, !17104, !17103, !17102, !17101, !17100, !17099, !17098, !17097, !17096, !17095, !17094, !17093, !17092, !17091, !17090, !17089, !17088, !17087, !17086, !17085, !17084, !17083, !17082, !17081, !17080, !17079, !17078, !17077, !17076, !17075, !17074, !17073, !17072, !17071, !17070, !17069, !17068, !17067, !17066, !17065, !17064, !17063, !17062, !17061, !17060, !17059

Count: **50**

## Review notes

Merged master MRs and substantive maintainer discussion were weighted above release backports, abandoned drafts, and mechanical/superseded changes.

Notable evidence in this batch includes:

- !17060 (merged master): Kerberos PKINIT updates. The MR supplied a reproducing capture and keytab. John Thacker explained that ASN.1-generated dissectors should be changed through the ASN.1/template/CNF inputs and regenerated using the build-system targets (for example `ninja generate_dissector-pkinit`, or the aggregate ASN.1 target), rather than hand-editing generated C or invoking `asn2wrs.py` ad hoc without the build-system import context. This reinforces existing generated-dissector/submission guidance.
- !17080 (merged master): initializes CPMv1 ITS private data and protocol/info columns before generated PER sequence dissection, reinforcing existing state-initialization guidance.
- !17070 (release-4.2 backport): release tests explicitly require Lua when `--enable-release` is selected even though Lua is optional for ordinary builds. This reinforces the distinction between generally optional features and required capabilities of official release/package configurations.
- !17108 and !17100 are release-4.2 null-check backports and therefore carry less independent architectural weight than their originating master changes.
- !17090 is a closed draft workaround for an `asn2wrs` constraint-handling limitation and was weighted below accepted changes.

No new convention file was added or modified in this run because the durable observations above reinforce guidance already represented in the notebook rather than establishing a sufficiently distinct new rule.

## Continuation

Rebuild the already-reviewed set from all tracking before the next run. If no higher-numbered gap is found, the next descending candidate is !17058.
