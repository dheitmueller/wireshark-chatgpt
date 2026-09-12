# Wireshark MR automation review: !21841-!21890

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit reviewed: `9e52bc78659a888d4eb624984ee1a886a40d959f`
Review direction: newest to oldest
MRs reviewed in this run: 50

Selection was built from the union of all existing `reviewed-mrs-automation/` ledgers and `reviewed-mrs.md`, rather than assuming numeric ranges were complete. The historical !17571-!17620 batch remains part of the already-reviewed set. No previously reviewed sparse exception occurred inside this batch, so the fifty highest-numbered unreviewed corpus entries were exactly !21890 through !21841.

## Exact reviewed MR set

- !21890
- !21889
- !21888
- !21887
- !21886
- !21885
- !21884
- !21883
- !21882
- !21881
- !21880
- !21879
- !21878
- !21877
- !21876
- !21875
- !21874
- !21873
- !21872
- !21871
- !21870
- !21869
- !21868
- !21867
- !21866
- !21865
- !21864
- !21863
- !21862
- !21861
- !21860
- !21859
- !21858
- !21857
- !21856
- !21855
- !21854
- !21853
- !21852
- !21851
- !21850
- !21849
- !21848
- !21847
- !21846
- !21845
- !21844
- !21843
- !21842
- !21841

## Durable findings promoted

- !21861 — merged master, authored and merged by Guy Harris. Route callers through the stable semantic `report_*` facade rather than lower-level failure/alert presentation routines; make implementation helpers private so reporting internals can evolve without expanding the API contract. Promoted to `api-design-conventions.md`.
- !21884 + !21890 — merged master correction chain by John Thacker, with the cross-platform failure reported by Stig Bjørlykke. Hiding `DataSourceTab` during tab removal avoids repeated expensive layout work, but removal of the explicit `clear()` changed lifecycle behavior on macOS. Performance optimizations around Qt containers must preserve explicit cleanup semantics and be validated across supported platform backends. Promoted to `platform-gui-conventions.md`.
- !21856 — merged master, authored by John Thacker and merged by Michael Mann. Distinct user-facing filter semantics require distinct `hf_` fields even when values/types overlap; generated correlation fields must not broaden the semantics of a packet-carried field. Promoted to `protocol-field-semantic-conventions.md`.
- !21852, !21858, !21860 — merged master generated-dissector cleanup, with explicit Michael Mann guidance in !21860 to minimize manual intervention. Metadata derivable from authoritative generated inputs should be generated rather than maintained as a parallel hand-written copy. Promoted to `generated-code-conventions.md`.

## Strong corroborating evidence retained without duplicate notebook rules

- !21886, !21874, !21847, !21844 — merged OSS-Fuzz/signed-overflow fixes reinforce checked offset arithmetic, semantic unsigned types for unsigned wire fields, and use of TVBuff remaining-length/checking helpers instead of overflow-prone `offset + length` expressions.
- !21871 and !21867 — merged Michael Mann refactors put RTP/Bluetooth information needed by consumers into tap data instead of requiring UI/application code to call protocol-internal APIs; reinforces the established application-layer/tap boundary rule.
- !21876 — Stig Bjørlykke review explicitly rejects fixing only generated Kerberos output and requires the corresponding template/source update; reinforces the existing generated-source-of-truth rule.
- !21889 — first-contributor dissector correction includes a focused Ubiquiti sample capture; reinforces existing sample-capture validation guidance.
- !21888 — TLS state fix by Stig Bjørlykke seeks targeted review from Peter Wu for TLS expertise; corroborates routing specialized protocol/security changes to domain experts.
- !21882 — CI conflict-check mode promotes findings to `ws_error`; corroborates the principle that validation deliberately enabled for CI should be enforceable rather than advisory only.
- !21869 — Windows support logic uses the concrete OS build required by the API baseline and keeps documentation/installer checks aligned; corroborates existing platform-minimum policy guidance.
- !21866 — John Thacker catches a distro-specific Qt6 development package name difference during setup-script review; reinforces portability checks across supported packaging ecosystems.
- !21862 and !21855 — preserve installed include-directory namespaces to avoid collisions such as Wireshark's `endian.h` versus libc's header; corroborates public-header/install-layout guidance.
- !21845, plus release backports !21849 and !21850 — Guy Harris replaces incomplete hand-maintained Windows exception strings/static storage with the platform's authoritative `ntdll.dll` messages and a thread-safe result path. Useful platform/library design evidence, but not promoted as a separate general rule in this run.
- !21842 — merged ASTERIX fuzz fixes/refactoring and unique filter-name cleanup reinforce existing fuzzing and field-registration guidance.

All other MRs in the exact set above were reviewed for discussion, outcome, and diff significance. Routine documentation, backport-only, spelling, packaging, narrow protocol-value, or mechanically repetitive changes were retained in the reviewed set but did not justify new notebook conventions.