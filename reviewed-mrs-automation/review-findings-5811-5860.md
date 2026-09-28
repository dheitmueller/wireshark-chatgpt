# Review findings: !5811–!5860

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Evidence weighting: merged master changes and accepted maintainer review are strongest; stable backports corroborate; !5859/!5852 are closed and !5815 is still open, so those are not accepted implementation precedent.

| MR | Result |
|---|---|
| !5860 | Merged. Extrememesh buffer sizing now uses the actual source/destination address lengths and avoids allocation when addresses are absent. |
| !5859 | Closed draft. John Thacker rejects protocol-enable state as the Decode As discriminator; superseded by later merged !5950. |
| !5858 | Merged by John Thacker. Even a systemd Journal Export Block with no options needs Wiretap block-type registration; Guy Harris also requests systemd-specific naming and separate scope for an unrelated UI issue. |
| !5857 | Merged by John Thacker. Text2pcap round-trip tests use `--hexdump frames` so decrypted/reassembled secondary data sources do not change the bytes under test. |
| !5856 | Merged by Gerald Combs. CMake discovers Sparkle's framework version and requires Sparkle 1 exactly until the Sparkle 2 API is supported. |
| !5855 | Merged. Adds Vector custom pcapng block handling; Stig Bjørlykke requests an anonymized sample and notes the intended architectural move away from frame-level custom-block dissection. |
| !5854 | Merged. 3GPP Supported Features handling in JSON; protocol-specific. |
| !5853 | Merged; strong Guy Harris review. pcapng IDBs need not immediately follow an SHB. Streaming readers/writers may discover link-layer metadata late and cannot assume full-file knowledge. |
| !5852 | Closed. Jaap Keuter asks the contributor to use a dedicated topic branch instead of fork `master`; replaced by a new MR. |
| !5851 | Merged release-3.4 backport of !5849. |
| !5850 | Merged release-3.6 backport of !5849. |
| !5849 | Merged master. Standards-permitted zero-length RSL Cause IE returns cleanly instead of forcing a malformed value; sample capture supplied. |
| !5848 | Merged release-3.4 backport of !5840. |
| !5847 | Merged release-3.6 backport of !5840. |
| !5846 | Merged by Pascal Quantin. NGAP state observed across TRY/non-local-jump handling is marked `volatile`; ASN.1 config and generated output stay synchronized. |
| !5845 | Merged by Guy Harris. Clarifies which exception subsystem state is truly per-thread and which callbacks remain process-wide. |
| !5844 | Merged release-3.6 backport of !5843, authored by Guy Harris. |
| !5843 | Merged. QApplication desktop file identity is set explicitly to match the installed `.desktop` basename. |
| !5842 | Merged. Ruckus RADIUS dictionary refresh; data maintenance. |
| !5841 | Merged. New coloring rules are enabled automatically; UI-local behavior. |
| !5840 | Merged with reproducer. PROXY v2 TLV parsing stops at the declared header end rather than parsing payload bytes as more TLVs. |
| !5839 | Merged by John Thacker. Text import permits synthetic IP/L4 headers with Raw IP encapsulations without inventing Ethernet and validates header/encapsulation compatibility in the shared engine. |
| !5838 | Merged. CFM 1SL PDU support; protocol-specific. |
| !5837 | Merged. SSH translation-unit-local debug helpers become `static`; linkage cleanup. |
| !5836 | Merged. MPEG registration identifier table expansion. |
| !5835 | Merged by Guy Harris. Registration-worker exceptions are caught in the worker, their messages copied before `ENDTRY`, transferred through join, and rethrown by the controlling initialization thread. |
| !5834 | Merged release-3.4 backport of !5832. |
| !5833 | Merged release-3.6 backport of !5832. |
| !5832 | Merged. MPLS ECHO FEC-stack offset/remaining-length accounting corrected. |
| !5831 | Merged by John Thacker. ISO timestamp-format keyword becomes case-insensitive and NULL-safe. |
| !5830 | Merged by John Thacker. IPv6 documentation/examples use the RFC 3849 documentation prefix. |
| !5829 | Merged by Guy Harris. Wireshark's exception catcher stack becomes thread-local, fixing cross-thread jump-state corruption. |
| !5828 | Merged by Gerald Combs. Restores an SSH NULL check after an earlier review request was found to rely on a mistaken reading of GLib behavior. |
| !5827 | Merged fuzz fix. Packet-driven SSH bignum size is capped and an expert diagnostic reports values beyond the chosen practical limit. |
| !5826 | Merged. PTP replaces an OUI literal with the shared ITU-T OUI definition. |
| !5825 | Merged. Adds IEEE 802.1AS-2020 one-step Sync support; protocol-specific. |
| !5824 | Merged by John Thacker. Import-from-Hex-Dump final execution validates that the selected dummy-header type is valid for the current encapsulation despite stale UI selection state. |
| !5823 | Merged. Adds PTP cross-packet timing analysis; later discussion asks for additional PDelay samples. |
| !5822 | Merged. SIGNAL-PDU aggregation features; feature-specific. |
| !5821 | Merged. DVB/MPEG length fields use consistent decimal presentation. |
| !5820 | Merged automatic update; no new human-review convention. |
| !5819 | Merged release-3.6 automatic update; no new lesson. |
| !5818 | Merged release-3.4 automatic update; no new lesson. |
| !5817 | Merged static-analysis cleanup. Jaap Keuter suggests guaranteeing string out-parameter initialization in the helper itself; useful corroboration, though the accepted diff remains caller-side initialization. |
| !5816 | Merged SFTP dissector. Jörg Mayer supports review-friendly commit separation when meaningful; Pascal Quantin warns GitLab automatic squash can be lost during Wireshark utility rebases and recommends manual squashing before merge; Alexis requests a component-prefixed subject. |
| !5815 | Still-open draft; down-weighted. Cross-platform testing found compiler/style issues in a generic dialog text-search proposal, but its architecture is not accepted precedent. |
| !5814 | Merged release-3.4 backport of !5811. |
| !5813 | Merged release-3.6 backport of !5811. |
| !5812 | Merged by John Thacker. Synthetic IPv6 packets use RFC 4193 ULA defaults; discussion distinguishes runtime dummy traffic from RFC 3849 documentation examples. |
| !5811 | Merged master. Adds DVB-defined MPEG-TS PID descriptions, with stable backports. |

Strongest durable evidence: !5853, !5857, !5856, !5840, !5835, !5829, !5827, !5824, and the submission discussion in !5816.
