# Wireshark MR Review Findings: !8862–!8911

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs in descending order, !8911 through !8862. Status mix: **49 merged, 1 closed/unmerged (!8891), 0 open**. Merged master work and substantive maintainer review were weighted most strongly; stable backports mainly corroborate their master changes; the superseded !8891 was down-weighted.

## Strongest durable findings

- **!8901 — dynamic registered fields follow configuration lifetime, not capture-file lifetime.** HTTP/2 previously tore down and recreated UAT-defined fields on file close/open. John Thacker moved replacement to UAT post-update/reset callbacks because columns, coloring rules, and filters can retain field references across capture-file transitions. This is early concrete evidence for the later, stronger deferred-deregistration lifecycle rules already in the notebook.
- **!8907 — prefer the common checksum helper when its semantics fit.** Jaap Keuter and Alexis La Goutte explicitly requested `proto_tree_add_checksum()`; the merged result centralizes checksum value, verification status, expert reporting, and calculated comparison. The contributor supplied focused captures for checksum-present and checksum-absent cases.
- **!8895/!8896/!8897 — registered masks are observable machine-output semantics.** John Thacker moved S1AP, NGAP, and XNAP cell-identity masks into the `hf_` declarations and used ordinary `proto_tree_add_item()`. The MR descriptions explicitly call out incorrect logical values in JSON/PDML from the previous manual extraction/add-value path.
- **!8876/!8874/!8875/!8885/!8898 — keep Werror for project code while isolating generator-owned warnings.** The series enables Werror broadly, then confines Flex/Lemon warning suppression to generator-owned regions rather than weakening checks for handwritten code.
- **!8877 — source range and semantic string value are separate contracts.** John Thacker explicitly distinguished including a line terminator in the packet-highlight/source range from including it in the decoded field string.
- **!8863 → !8868 — a container refactor must preserve absence/nullability.** Moving X.509 DN construction to `wmem_strbuf` removes unsafe byte truncation, but the accessor still needs to handle the not-yet-created state.
- **!8887 — static-analysis fixes that alter protocol semantics need domain validation.** Martin Mathieson did not mechanically apply cppcheck's PTP finding; he asked protocol expert Lars Völker to confirm the intended modulo correction, and Lars confirmed it.
- **!8884 — when standards and deployed encodings conflict, document both and make the heuristic explicit.** João Valverde corrected the initial standards interpretation; the merged code documents RFC 5149 UTF-8 and deployed 3GPP APN-style encoding separately before choosing a heuristic.

## Qualified and corroborating evidence

!8910 is useful early evidence for parser-driven display-filter autocomplete, but later merged !23146/!23147 provide the stronger mature rule. !8894, !8890, !8889, !8888, and !8869 reinforce existing UTF-8/text-sanitization guidance. !8908 directly motivates the broader Werror work. !8862 is useful sequence-number evidence but the same MR points to later !8988 for further correction. !8911, !8906, and !8893 are temporary CMake 3.24.3 pins around a CMake 3.25 behavior change; !8883, !8870, !8866, and !8865 show build/CI policy following supported OS/toolchain baselines.

Closed !8891 is not accepted implementation evidence. Anders Broman records that it was superseded by !17660; it contributes only low-weight submission/process evidence.

## Complete batch inventory

| MR | Outcome | Review depth / result |
|---|---|---|
| !8911 | merged | Scanned; release-3.6 backport of the temporary CMake 3.24.3 pin. |
| !8910 | merged | Deep; parser/compiler-driven autocomplete using structured syntax error state. |
| !8909 | merged | Scanned; Qt log-handler severity/context cleanup. |
| !8908 | merged | Discussion-focused; Clang warnings caught in review, motivating Werror coverage. |
| !8907 | merged | Deep; standard checksum helper requested by Jaap Keuter and Alexis La Goutte. |
| !8906 | merged | Scanned; release-4.0 backport of the CMake pin. |
| !8905 | merged | Scanned; Qt6 RTP-player pause/resume timer fix, later backport requested. |
| !8904 | merged | Scanned; automatic data/translation update. |
| !8903 | merged | Scanned; release-3.6 automatic data update. |
| !8902 | merged | Scanned; release-4.0 automatic data update. |
| !8901 | merged | Deep; UAT dynamic-field lifecycle moved away from capture-file lifecycle. |
| !8900 | merged | Discussion-focused; selected-row crash guard; naming feedback was not the final architectural outcome. |
| !8899 | merged | Corroborating; expands Werror coverage to sharkd. |
| !8898 | merged | Deep; generator-specific Flex/Lemon diagnostic scopes. |
| !8897 | merged | Deep; XNAP registered bitmasks and machine-output semantics. |
| !8896 | merged | Deep; NGAP registered bitmasks and machine-output semantics. |
| !8895 | merged | Deep; S1AP registered bitmask and machine-output semantics. |
| !8894 | merged | Deep/corroborating; validate/repair URI-unescaped text before FT_STRING storage. |
| !8893 | merged | Scanned; master CMake 3.24.3 pin. |
| !8892 | merged | Scanned; QCustomPlot vendor update with local fixes reapplied. |
| !8891 | closed/unmerged | Discussion-focused; superseded by !17660, so down-weighted. |
| !8890 | merged | Corroborating; single-byte IAX2 character formatting made valid UTF-8. |
| !8889 | merged | Corroborating; AT command raw bytes pass through format_text(). |
| !8888 | merged | Corroborating; T.30 text conversion uses the UTF-8 conversion path. |
| !8887 | merged | Deep; static-analysis semantic fix confirmed with protocol expert. |
| !8886 | merged | Scanned; removes obsolete release-note entry. |
| !8885 | merged | Corroborating; Flex-generated documentation warning suppression. |
| !8884 | merged | Deep; RFC/3GPP encoding conflict documented and heuristically distinguished. |
| !8883 | merged | Scanned; CMake minimum raised to supported-platform baseline. |
| !8882 | merged | Scanned; vendored QCustomPlot documentation warning cleanup. |
| !8881 | merged | Scanned; release-4.0 locamation fixes backport. |
| !8880 | merged | Corroborating; old warning exceptions re-evaluated after Release CI coverage. |
| !8879 | merged | Scanned; vendored QCustomPlot documentation warning cleanup. |
| !8878 | merged | Corroborating; AOL token represented as an ASCII string field. |
| !8877 | merged | Deep; source/highlight extent separated from semantic string extent. |
| !8876 | merged | Deep; enables Werror in Clang CI. |
| !8875 | merged | Corroborating; Lemon-generated unreachable warning isolated. |
| !8874 | merged | Deep/corroborating; Werror restricted to nongenerated sources where required. |
| !8873 | merged | Corroborating; enables Werror for tfshark and fixes resulting warnings. |
| !8872 | merged | Scanned; compiler-flag cleanup, with later post-merge discussion about custom linker flags. |
| !8871 | merged | Scanned; typo only. |
| !8870 | merged | Scanned; release-4.0 removes obsolete/conflicting cmake3 package install. |
| !8869 | merged | Corroborating; explicit UTF-8 and TVBuff line helpers replace raw-byte strings. |
| !8868 | merged | Deep; X509IF accessor preserves NULL state after strbuf refactor. |
| !8867 | merged | Scanned; broad warning cleanup; no separate durable rule promoted. |
| !8866 | merged | Scanned; master removes obsolete/conflicting cmake3 package install. |
| !8865 | merged | Scanned; EOL openSUSE 15.3 CI jobs disabled on release-3.6. |
| !8864 | merged | Scanned; O-RAN modulation-compression alignment changes. |
| !8863 | merged | Deep; X509IF fixed buffers replaced by wmem_strbuf to avoid UTF-8 truncation. |
| !8862 | merged | Qualified/corroborating; wraparound-aware TCP sequence comparison, later refined by !8988. |
