# Automated MR review ledger: !10000-!10049

Corpus repository: `dheitmueller/wireshark-corpus-mrs`
Corpus commit: `08420a2f7851841b7a38714f642987c8cd167184`
Direction: descending MR number (newest available unreviewed toward older)
Count: 50

Reviewed MRs (exact set):

!10049, !10048, !10047, !10046, !10045, !10044, !10043, !10042, !10041, !10040,
!10039, !10038, !10037, !10036, !10035, !10034, !10033, !10032, !10031, !10030,
!10029, !10028, !10027, !10026, !10025, !10024, !10023, !10022, !10021, !10020,
!10019, !10018, !10017, !10016, !10015, !10014, !10013, !10012, !10011, !10010,
!10009, !10008, !10007, !10006, !10005, !10004, !10003, !10002, !10001, !10000.

Selection was made from the explicit already-reviewed set represented by the automation ledgers and `reviewed-mrs.md` where applicable; numeric gaps outside this exact set are not implied reviewed. The historical !17571-!17620 batch remains part of the already-reviewed set.

Notable durable evidence from this batch includes merged !10048 (const-correct API contract and compile-time-sized static string arrays for Exported PDU tags), with closed !10049 down-weighted because Guy Harris folded the corrected approach into !10048; merged !10020 (packet-scope `wmem_strconcat` replacing leaking GLib allocation); and merged !10000 (dumpcap child logging must respect sync/data-pipe framing, and a `va_list` reused by multiple consumers requires `va_copy`).

Next candidate after rebuilding the complete reviewed set: !9999, if present in the corpus.
