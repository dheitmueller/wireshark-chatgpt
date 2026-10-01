# Wireshark MR Review Findings — !3061–!3110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master changes receive the most weight; stable backports mainly corroborate them. Closed !3106 is review history, not accepted-code precedent.

| MR | Outcome | Finding |
|---|---|---|
| !3110 | merged | ERF USERAPPL stable backport of !3108. |
| !3109 | merged | ERF USERAPPL stable backport of !3108. |
| !3108 | merged | Guy Harris: keep USERAPPL when version is known but application name is not; use an explicit unknown-name placeholder. |
| !3107 | merged | João Valverde: rename always-on ws_assert_bounds() to behavior-oriented ws_abort_if_fail(); distinguish it from disableable assertions. |
| !3106 | closed | NSH None proposal; Anders Broman flagged author/rebase/submission issues; later implemented by !3837, so down-weighted. |
| !3105 | merged | NetScaler early-read cleanup stable backport. |
| !3104 | merged | NetScaler early-read cleanup stable backport. |
| !3103 | merged | Guy Harris: free allocated read buffer on initial-read failure/NOT_MINE path. |
| !3102 | merged | TACACS failed-conversion cleanup stable backport. |
| !3101 | merged | TACACS failed-conversion cleanup stable backport. |
| !3100 | merged | Guy Harris: when str_to_ip() fails, caller still owns addr_data and must free it before using fallback data. |
| !3099 | merged | DNP rejected-buffer cleanup stable backport. |
| !3098 | merged | DNP rejected-buffer cleanup stable backport. |
| !3097 | merged | Guy Harris: if CRC failure prevents buffer adoption as TVBuff backing storage, ownership never transfers; free it. |
| !3096 | merged | WSDG artwork-link maintenance; no durable convention. |
| !3095 | merged | Historical LTO/IPO default enablement; later reviewed !3121 is stronger/current policy evidence because it makes LTO opt-in. |
| !3094 | merged | NVMe ANA fix: retrieve protocol count, advance by actual field widths, and bound namespace iteration by that count. |
| !3093 | merged | John Thacker: give GSE a bounded subset TVBuff and standard dissector_t signature so carrier framing is separate from child decoding. |
| !3092 | merged | Automatic update; no durable convention. |
| !3091 | merged | OID loop/list cleanup stable backport. |
| !3090 | merged | Automatic update; no durable convention. |
| !3089 | merged | OID loop/list cleanup stable backport. |
| !3088 | merged | Automatic update; no durable convention. |
| !3087 | merged | Guy Harris: remove stray loop-breaking break and make linked-list head/tail transitions explicit to readers and analyzers. |
| !3086 | merged | EPL groups-vector cleanup stable backport. |
| !3085 | merged | EPL groups-vector cleanup stable backport. |
| !3084 | merged | Guy Harris: g_key_file_get_groups() result is g_mallocated string-vector data and requires g_strfreev(). |
| !3083 | merged | Guy Harris: allocate RTP IDs only after prerequisites; track transfer to output container and free untransferred objects. |
| !3082 | merged | Safe-save temporary-path cleanup stable backport. |
| !3081 | merged | Safe-save temporary-path cleanup stable backport. |
| !3080 | merged | Guy Harris: successful rename does not free the allocated temporary pathname; release it after rename. |
| !3079 | merged | Guy Harris: shared failure epilogue owns unlink; avoid duplicate cleanup and retain the primary export error over secondary close errors. |
| !3078 | merged | Export-abort cleanup stable backport. |
| !3077 | merged | Export-abort cleanup stable backport. |
| !3076 | merged | Guy Harris: abort path must unlink temporary output and free its allocated pathname. |
| !3075 | merged | Guy Harris: annotate printf-like helper with G_GNUC_PRINTF; compiler caught a real missing argument; make scratch buffer automatic for thread safety. |
| !3074 | merged | 802.11az draft update; accepted despite lack of packets. Useful standards-evolution history, but later final-standard validation evidence is stronger. |
| !3073 | merged | Guy Harris: store DOF session key as typed FT_BYTES with explicit length and SEP_COLON metadata instead of a manually allocated formatted string. |
| !3072 | merged | ErlDP fragmentation: Anders Broman caught integer narrowing, Gerald Combs caught guint64 format portability, Alexis La Goutte caught overlapping value-string semantics. |
| !3071 | merged | Fuzzshark error-string cleanup stable backport. |
| !3070 | merged | Fuzzshark error-string cleanup stable backport. |
| !3069 | merged | Guy Harris: init_progfile_dir() failure text is allocated; printing it does not release ownership. |
| !3068 | merged | Protobuf directory-handle cleanup stable backport. |
| !3067 | merged | Protobuf directory-handle cleanup stable backport. |
| !3066 | merged | Guy Harris: recursive protobuf loader closes directory handle before every child-load failure return. |
| !3065 | merged | Protobuf path/status-code cleanup stable backport. |
| !3064 | merged | Protobuf path/status-code cleanup stable backport. |
| !3063 | merged | Guy Harris: free constructed path on load failure and explicitly test a return code against 0 instead of treating a multivalue status as Boolean. |
| !3062 | merged | Guy Harris: ENC_APN_STR parses structural label-length octets as bytes and decodes only label payload, with bounds checks and replacement handling. |
| !3061 | merged | Zigbee Info labels changed from Dst/Src to spec terms Responder/Originator. |
