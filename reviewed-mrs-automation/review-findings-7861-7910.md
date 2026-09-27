# Review findings: Wireshark !7861-!7910

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is weighted above closed work; Guy Harris and other core-maintainer guidance receives the greatest weight.

| MR | Outcome | Depth | Finding |
|---|---|---|---|
| !7910 | merged | Scanned | MSYS2 setup switches Qt dependencies to Qt6. |
| !7909 | merged | Scanned | RPM setup/package build gains explicit Qt5/Qt6 selection. |
| !7908 | merged | Deep | DTLS validates malformed `use_srtp` vector lengths and reports expert info. |
| !7907 | merged | Deep | HTTP validates a candidate first-header name before claiming ambiguous continuation data. |
| !7906 | merged | Scanned | Qt address formatting initializes unsupported address types safely. |
| !7905 | merged | Discussion | OCP.1 backport; short-name naming-convention follow-up identified. |
| !7904 | merged | Deep | GCC/Qt warning suppression is narrowly scoped around the third-party include. |
| !7903 | merged | Discussion | Debian setup gains Qt5/Qt6 dependency modes; Qt Multimedia optionality discussed. |
| !7902 | merged | Deep / Guy review | Timestamp limit renamed for the actual 32-bit-seconds constraint and derived from storage width. |
| !7901 | merged | Deep / Guy | Endpoint-table terminology cleanup/backport. |
| !7900 | merged | Deep / Guy | Broad host-to-endpoint naming cleanup matching the data model. |
| !7899 | merged | Scanned / Guy | Endpoint-table comments corrected to use endpoint terminology. |
| !7898 | merged | Scanned / Guy | Master counterpart of !7899. |
| !7897 | merged | Deep | TCP OOO reassembly advances `maxnextseq` across all newly contiguous stored fragments. |
| !7896 | merged | Scanned / Guy | Deprecated API annotation points to the real replacement. |
| !7895 | merged | Scanned | Debian setup adds missing optional packages. |
| !7894 | merged | Scanned | Obsolete Lintian suppression removed. |
| !7893 | merged | Discussion | ABI failures after Ubuntu 22.04 investigated as compiler/DWARF/tool-version interaction. |
| !7892 | merged | Discussion / Guy | Broken ABI gate temporarily disabled while preserving its definition. |
| !7891 | merged | Scanned / Guy | Master counterpart of !7896. |
| !7890 | merged | Scanned | BSD setup switches to Qt6. |
| !7889 | closed | Low | Unneeded release-branch DWARF workaround; not authoritative. |
| !7888 | merged | Scanned | Release-branch DWARF-4 workaround for Valgrind. |
| !7887 | merged | Deep | Valgrind fuzz CI pins DWARF-4 because selected Valgrind cannot read Clang 14 DWARF-5. |
| !7886 | merged | Scanned | TShark-only jobs drop irrelevant Qt selection. |
| !7885 | merged | Deep / John review | TLS computes CCM AAD length once instead of duplicating version logic in two stages. |
| !7884 | merged | Scanned | Wi-SUN FAN specification update. |
| !7883 | merged | Deep | DoIP parses compatible unknown/newer versions instead of rejecting only on version byte. |
| !7882 | merged | Deep | Restores an omitted public compatibility declaration after endpoint API rename. |
| !7881 | merged | Scanned | ABI checker pins DWARF-4; branch counterpart. |
| !7880 | merged | Deep | ABI checker pins DWARF-4 for abi-dumper 1.2 compatibility. |
| !7879 | merged | Discussion | Valgrind DWARF workaround coordinated with container and Qt6 updates. |
| !7878 | merged | Deep | Master counterpart of narrow Qt-header warning suppression. |
| !7877 | merged | Scanned | Master counterpart of Qt address initialization fix. |
| !7876 | merged | Scanned | Fixes malformed JSON in CMake-preset documentation. |
| !7875 | closed | Low | Exploratory TCP OOO draft; merged !7897 is stronger evidence. |
| !7874 | merged | Deep / Guy | Endpoint-table API rename preserves deprecated wrappers and typedefs for source/binary compatibility. |
| !7873 | merged | Scanned / Guy | BLF terminology and comments clarified. |
| !7872 | merged | Scanned | NSIS removes obsolete Quick Launch option. |
| !7871 | merged | Deep | BLF accepts additional object-header formats used by newer files. |
| !7870 | merged | Scanned | Fedora RPM CI explicitly remains on Qt5. |
| !7869 | merged | Deep / Guy | Master endpoint-table migration separates endpoints from conversation/circuit identifiers and preserves ABI metadata. |
| !7868 | merged | Scanned | Ubuntu/RPM CI explicitly selects Qt5 after Qt6 default change. |
| !7867 | merged | Scanned | TCP Info displays SACK-permitted as presence, not a synthetic value. |
| !7866 | closed | Discussion / negative | João rejects unjustified GnuTLS find-module rewrite that removed pkg-config semantics and misread Windows hints. |
| !7865 | merged | Discussion | Gcrypt discovery adjusted for contributor environment; limited general authority. |
| !7864 | merged | Discussion | Qt6 docs review catches required-vs-optional wording and package-name drift. |
| !7863 | merged | Scanned | Windows Qt5 CI explicitly disables Qt6. |
| !7862 | merged | Deep | Qt6 becomes source-build default while conservative jobs opt into Qt5; setup/docs follow. |
| !7861 | merged | Deep | SMTP stores ordered per-PDU state with end offsets because one frame can switch command/data state repeatedly. |
