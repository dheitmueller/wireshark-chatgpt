# Wireshark MR review automation ledger: !19990-!20039

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit reviewed: `a3b98766b09f6822e4f2d38d2925205c0ee92383`

Selection method: rebuilt the already-reviewed set from the available files under `reviewed-mrs-automation/` and `reviewed-mrs.md`, preserving and counting the historical !17571-!17620 batch. The corpus commit is unchanged from the preceding run, so there were no newly populated higher-numbered corpus holes. The fifty highest-numbered corpus MRs not already present in the reviewed set were therefore !20039 through !19990 inclusive. No assumption about a numeric range was used in place of the explicit tracking set.

Exactly 50 MRs were reviewed in this run:

- !20039
- !20038
- !20037
- !20036
- !20035
- !20034
- !20033
- !20032
- !20031
- !20030
- !20029
- !20028
- !20027
- !20026
- !20025
- !20024
- !20023
- !20022
- !20021
- !20020
- !20019
- !20018
- !20017
- !20016
- !20015
- !20014
- !20013
- !20012
- !20011
- !20010
- !20009
- !20008
- !20007
- !20006
- !20005
- !20004
- !20003
- !20002
- !20001
- !20000
- !19999
- !19998
- !19997
- !19996
- !19995
- !19994
- !19993
- !19992
- !19991
- !19990

Review weighting: merged master MRs and substantive maintainer review were weighted most heavily; stable backports were treated as corroboration of their master changes; open drafts, abandoned/closed predecessors, and superseded work were down-weighted. In particular, closed !20038 and !20022 and draft !20019 were not treated as equivalent evidence to accepted master work.

Durable notebook updates from this batch:

- !20029, corroborated by stable backport !20032: pointer-to-pointer out parameters require storage of the API's actual pointer-sized output type; receive first, then explicitly convert to a scalar representation. The original SSH misuse could segfault on the SFTP sample capture.
- !20031: direct Guy Harris review recommends `ws_strtou32()` over generic/local textual numeric conversion because the Wireshark helper supplies additional error and range checking.
- !19990: Guy Harris-authored Wiretap work carries the owning file-type discriminator with file-type-specific records, validates it at write time, and provides useful diagnostic information with `WTAP_ERR_UNWRITABLE_REC_TYPE`.

Additional strong corroborating evidence included Guy Harris-authored !20034 for canonical padding helpers, John Thacker's !19996/!19997/!19998 series for honoring helper return-value coordinate semantics, and the !20021 -> !20027/!20028 correction sequence showing that encoding flags must not be mechanically normalized when they carry API-specific bit-order semantics.

Notebook commits made before this ledger:

- `42ea0b0837b374b37ec5f59d02c0d6017719c712` — Document out-parameter storage type convention.
- `f26b5304a887983b7dbd5af849a550eb05b20f9e` — Add checked textual conversion guidance.
- `3a418acde44c0c4ec7def3e569de1f95e017d6f1` — Document domain identity for format-specific records.
