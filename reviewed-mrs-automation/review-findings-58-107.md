# Review findings — Wireshark !58–!107

Corpus revision: `ddcaa22b51c68f594e425a23388c3a2086813054`. Merged master work is weighted above stable backports; closed/superseded proposals are used only for review/process evidence.

## Strongest findings

- **!91 + !103:** Guy Harris gives the authoritative rationale for distinguishing Visual Studio CRT UTF-8 locale from Windows ANSI APIs and console encodings. !103 is the immediate corrective final code state.
- **!87:** a misspelled preference key is intentionally retained because renaming it would lose existing saved settings. Persisted preference identifiers require migration/aliasing, not cosmetic renaming.
- **!96 + !98–!100:** packet-scoped wmem storage must not be individually `g_free()`d.
- **!84 + !86/!88/!102:** registered field metadata constrains valid generated values; an arbitrary status outside a `BASE_NONE` field's table can assert.
- **!80:** validate packet-controlled range ordering before unsigned subtraction or container growth.
- **!73:** Anders Broman explicitly prefers `proto_tree_add_item_ret_uint/int` when the displayed field also drives code; Alexis La Goutte asks for registered bitmask semantics.
- **!60:** exact-prefix semantics belong in the generator; Python `lstrip` was the wrong primitive and corrupted NAN field abbreviations.
- **!90:** FCoE FCS inference uses EOF plus padding position as corroborating framing evidence.
- **!85:** reusing pointer-bearing working structs can accidentally share ownership and double-free; allocate fresh per-record mutable subobjects after append.
- **!58 → !65:** CIP state consolidation was functionally neutral but broke macOS clang; the corrective build-portability change is part of the accepted final state.
- **!107 / !101 / !64 / !88 / !95:** maintainers treat MR branch history, rebase shape, commit subject, and maintainer-edit permission as part of submission quality.

## Supporting observations

!81 keeps ASN.1 template and generated NGAP output synchronized. !83 guards a zero-length IE before field access. !82 installs the directly required CI dependency instead of a larger umbrella package. !89 isolates ccache per job and reports measured build-time improvements. !67–!70 are automatic registry refreshes with little durable review guidance. The large spelling sweeps !63, !71, !74, !75, !79, and !94 are historical cleanup evidence but are not strong current precedent for renaming public filter identifiers.

## Per-MR disposition

| MR | Outcome | Title |
|---:|---|---|
| !107 | merged | tools: Force "Allow commits from members..." in merge requests. |
| !106 | merged | Remove tools/commit-msg and migrate commit validation. |
| !105 | merged | gitlab-ci: Enable the Windows MR build. |
| !104 | closed / unmerged | Fix windows build |
| !103 | merged | Fix the Windows build. |
| !102 | merged | TCP: do not use an unknown status when the checksum is 0xffff |
| !101 | merged | Qt: Use UTF8 middle dot for non-printable characters |
| !100 | merged | multipart: fix deallocation of invalid parts |
| !99 | merged | multipart: fix deallocation of invalid parts |
| !98 | merged | multipart: fix deallocation of invalid parts |
| !97 | closed / unmerged | Draft: EPL: fixed size detection multiple r/w SDOs |
| !96 | merged | multipart: fix deallocation of invalid parts |
| !95 | merged | RTPS: Fixing typo in a mask, it should be app_id instead of host_id |
| !94 | merged | Fix some spelling mistakes found among plugins. |
| !93 | closed / unmerged | RTPS: Fixed a typo that made the Topic Information feature not work |
| !92 | closed / unmerged | PROFINET: IOCS and IOData object dissection with Multiple AR |
| !91 | merged | get_zonename(): don't convert _tzname[] values to UTF-8. |
| !90 | merged | FCOE: Autodetect Ethernet FCS by examining EOF |
| !89 | merged | GitLab CI: Set up ccache. |
| !88 | merged | TCP: do not use an unknown status when the checksum is 0xffff |
| !87 | merged | Fix some spelling errors detected in epan/prefs.c |
| !86 | merged | TCP: do not use an unknown status when the checksum is 0xffff |
| !85 | merged | USB HID: Fix a double free. |
| !84 | merged | TCP: do not use an unknown status when the checksum is 0xffff |
| !83 | merged | GTpv2: Add expert info for zero length IE |
| !82 | merged | CI+tools: Install lintian. |
| !81 | merged | NGAP: fix ngap.MDT_Location_Information.reserved definition |
| !80 | merged | USB HID: Avoid allocating a huge amount of memory. |
| !79 | merged | More spelling fixes, last part of 2nd pass of dissectors. |
| !78 | merged | Fix dist. |
| !77 | merged | cl3: (trivial) drop _U_ for a parameter that is used |
| !76 | closed / unmerged | cl3: (trivial) drop _U_ for a parameter that is used |
| !75 | merged | More spelling fixes, part 2 of 2nd pass of dissectors. |
| !74 | merged | More spelling fixes, part 2 of 2nd pass of dissectors. |
| !73 | merged | Portcontrol: Added support for option code 130 (RFC 7753) and updated info column |
| !72 | closed / unmerged | packet_mq: Support V9.2, improve MultiSegment, improve some struct display |
| !71 | merged | More spelling fixes, start of second pass of dissectors. |
| !70 | merged | Update numbers 2020 08 30 master 2.6 |
| !69 | merged | [Automatic update for 2020-08-30] |
| !68 | merged | [Automatic update for 2020-08-30] |
| !67 | merged | [Automatic update for 2020-08-30] |
| !66 | closed / unmerged | SSH decrypted data dissection |
| !65 | merged | Fix build where compilers can't initialise multi-field struct with {0} |
| !64 | merged | ITS: enable decoding of UDP datagram as ITS message |
| !63 | merged | Fix more spelling errors in dissector strings. |
| !62 | merged | ErlDP: support features of Erlang/OTP 23 |
| !61 | closed / unmerged | Draft: Update numbers 2020 08 28 master |
| !60 | merged | nl80211: Fix abbreviated field names for NAN |
| !59 | merged | EBHSCR: Add CAN and TS, update ETH dissectors |
| !58 | merged | CIP: Combine connection structs |
