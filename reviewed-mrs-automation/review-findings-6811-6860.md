# Wireshark MR review findings: !6811-!6860

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

All 50 MRs in this batch are merged. Merged work is treated as accepted evidence; maintainer-authored and direct maintainer review—especially Guy Harris guidance—receives extra weight.

| MR | Depth | Finding |
|---|---|---|
| !6860 | Scanned | Automatic release-3.4 registry/data update; no new durable convention. |
| !6859 | Deep / high-authority | Guy Harris makes Conversations/Endpoints sort resolved addresses by the resolved display text instead of the raw address when name resolution is enabled. |
| !6858 | Discussion-focused | Corrects duplicate IEEE 802.11 filter abbreviations. Alexis La Goutte asks about CI coverage; Martin Mathieson notes `check_typed_item_calls.py` found it and discusses warning-only checker flags. |
| !6857 | Scanned | Fuzz failure headers gain a UTC timestamp for better diagnostics. |
| !6856 | Scanned | CI temporarily returns from Clang 14 to Clang 12; environment maintenance only. |
| !6855 | Deep | Packet-list sorting operates on visible rows rather than every physical row, avoiding whole-capture sort work for filtered views. |
| !6854 | Scanned | Falco Bridge adds address fields; implementation-specific. |
| !6853 | Scanned | CIP Safety error-detection failures are raised from warning to error severity; protocol-specific. |
| !6852 | Deep | AT dissector keeps evolving session state in conversation data but snapshots pre-packet state in packet proto-data so redissection is deterministic; stateless fallback remains when no conversation exists. |
| !6851 | Scanned | Release-3.6 documentation corrects the minimum Qt version. |
| !6850 | Scanned | PFCP updated to 3GPP TS 29.244 V17.4.0; protocol-spec maintenance. |
| !6849 | Scanned | Release-3.6 documentation corrects minimum GLib and its download source. |
| !6848 | Scanned | Master documentation updates minimum GLib/Qt versions. |
| !6847 | Scanned | Stable documentation removes obsolete `configure` references. |
| !6846 | Deep | John Thacker makes RPM packaging work from `git archive` source trees by embedding/verifying commit provenance; documents a narrow ShellCheck false-positive suppression for intentional git-archive placeholder syntax. |
| !6845 | Scanned | CIP Safety recognizes Cancel Propose/Apply TUNID; protocol-specific. |
| !6844 | Scanned | CIP Safety expert fields are corrected; protocol-specific. |
| !6843 | Scanned | CIP Safety SERCOS III attributes corrected; protocol-specific. |
| !6842 | Deep | Gerald Combs makes generated configuration-file comments use the current product/configuration namespace instead of hard-coding Wireshark, completing Guy Harris's !6821 review follow-up. |
| !6841 | Scanned | sshdump documentation clarifies supported OpenSSH private-key format. |
| !6840 | Scanned | CI disables an irrelevant SAST analyzer; scanner selection maintenance. |
| !6839 | Scanned | Master documentation removes obsolete `configure` references. |
| !6838 | Deep | EAP feature work is moved to master per Alexis La Goutte's policy guidance; representative captures/keys are supplied, Windows CI catches an encoding portability error, and OSS-Fuzz later exposes an address-copy leak fixed by !6864. |
| !6837 | Scanned | Falco Bridge moves to the current libsinsp capabilities API. |
| !6836 | Scanned | Debian symbol metadata adds a missing exported symbol. |
| !6835 | Scanned | Fuzz-test error-header formatting and CI metadata improved. |
| !6834 | Scanned | PROFINET severity names updated to PA Profile 4.02; protocol-specific. |
| !6833 | Discussion-focused | A maybe-uninitialized warning is eliminated by benign initialization; João Valverde notes the supposedly uninitialized path requires a bogus protocol-layer invariant. |
| !6832 | Discussion-focused | Semgrep is disabled because it cannot reliably parse much of the dissector corpus and crashes on one source file; analyzer usefulness depends on trustworthy language coverage. |
| !6831 | Deep | Source-tarball reuse is accepted only when the archive's embedded commit ID matches the requested build revision. |
| !6830 | Scanned | Documentation parameter-name warning fixed. |
| !6829 | Deep | Compile-context-sensitive clang validation skips changed files for which the build has no rule, avoiding analysis without the required include/compile context. |
| !6828 | Scanned | Release-3.6 backport of NAS-5GS configuration-update correction. |
| !6827 | Scanned | GTP' Release Identifier Extension decoding corrected; protocol-specific. |
| !6826 | Scanned | NAS-5GS Configuration Update Command uses the correct IE tag; protocol-specific. |
| !6825 | Deep | Wi-SUN FAN 1.1 update. Alexis La Goutte requests a sample capture, catches formatting/typo issues, and explicitly prefers `pinfo->pool` over deprecated packet-scope access; submitter supplies encrypted capture plus keys. |
| !6824 | Deep | LLDP no longer treats a missing optional EndOfLLDPDU TLV as malformed; repeated TLV parsing is bounded by captured packet length rather than requiring a sentinel. |
| !6823 | Scanned | Falco Bridge tracks current libsinsp API and improves load-error propagation. |
| !6822 | Scanned | CMake populates the Falco plugin directory for Logwolf. |
| !6821 | Deep / review follow-up | Gerald Combs relocates shared resources; Guy Harris catches a Logwolf file incorrectly branded as generated by Wireshark, leading to !6842. |
| !6820 | Scanned | Release-3.6 RPM spec cleanup; backport of !6819. |
| !6819 | Scanned | RPM spec cleanup on master. |
| !6818 | Scanned | Stable RPM packaging handles SUSE 15.1 build-directory macro behavior. |
| !6817 | Scanned | Spelling checker cleanup and dictionary additions. |
| !6816 | Scanned | Release-3.6 802.11 TWT Setup dissection fix. |
| !6815 | Scanned | Release-3.4 802.11 TWT Setup dissection fix. |
| !6814 | Scanned | Release-3.6 RPM macros updated for RHEL 8 and SUSE handling. |
| !6813 | Deep / high-confidence | Peter Wu resets TLS/DTLS prior-epoch state before hashing the new Hello, fixing EMS renegotiation; John Thacker, Ivan Nardi, and Alexis La Goutte review positively, and the reporter confirms the fix before backport. |
| !6812 | Scanned | RPM comment expanded to document SUSE macro behavior. |
| !6811 | Scanned | Stable html2text gains quoting/suffix handling; tooling maintenance. |

## Highest-confidence durable evidence

The strongest reusable evidence is !6859 (Guy Harris: view ordering must match resolved display semantics), !6852 (packet-boundary snapshots make mutable conversation state redissection-safe), !6846/!6831 (source-archive provenance must survive operation outside a Git checkout), !6838 (new features on master, fixes/backports on release branches, with captures and cross-platform/fuzz validation), !6825 (representative captures plus `pinfo->pool` for packet-lifetime allocations), !6824 (optional terminators must not override bounded-container semantics), and !6813 (clear prior-epoch state before the first message of a new epoch is accumulated).
