# Review findings: !7661-!7710

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 previously unreviewed MRs, working downward from !7710. Merged work is treated as accepted evidence; closed/draft/superseded work is retained mainly for negative review or workflow guidance and is weighted below merged successors. Maintainer-authored and maintainer-reviewed changes, particularly Guy Harris guidance, receive additional weight.

| MR | Outcome | Weight | Finding |
|---|---|---|---|
| !7710 | merged | Scanned | TLS cipher lookup table synchronized with the ENC_* value domain; accepted successor to the earlier closed attempts. |
| !7709 | closed | Low / superseded | Earlier TLS cipher synchronization attempt; superseded by merged !7710. |
| !7708 | merged | Discussion-focused | SharkFest welcome-page links. Alexis La Goutte requested a squash for easier backporting; UI-specific otherwise. |
| !7707 | merged | Scanned | PFCP QUASF feature-bit typo correction; no broader convention. |
| !7706 | merged | Deep / high-authority | Extcap shutdown no longer treats child-process exit as proof stdout/stderr are closed. Session completion waits on process and channel watches; timeout removes lingering watches. Guy Harris supplied extensive cross-Unix pipe/select/poll portability analysis. |
| !7705 | merged | Scanned | TLS digest table synchronized with DIG_* definitions; straightforward table consistency fix. |
| !7704 | merged | Scanned | L2TP protocol-length accounting and bitmask presentation cleanup; protocol-specific usability improvement. |
| !7703 | merged | Scanned | TLS debug-text correction only. |
| !7702 | merged | Deep | John Thacker replaces Wireshark-private L2TP pseudowire dispatch numbers with the actual IANA pseudowire values carried on the wire, aligning normal dispatch and Decode As. |
| !7701 | closed | Discussion-focused / superseded | Alexis La Goutte asked that UNKNOWN stay last and told the contributor to amend and push the existing MR instead of opening a replacement. Accepted outcome is !7710. |
| !7700 | merged | Scanned | NR RRC ASN.1 dissector update to v17.1.0; generated/spec update without reusable review discussion. |
| !7699 | merged | Scanned | MySQL capability regression fix. Backport was explicitly judged unnecessary because the introducing change had not shipped in a release. |
| !7698 | closed | Low / superseded | Another TLS cipher synchronization attempt, closed after failed CI; superseded by !7710. |
| !7697 | merged | Scanned | LPP ASN.1 dissector update to v17.1.0; no new cross-cutting rule. |
| !7696 | merged | Deep | John Thacker fixes L2TP UDP conversation lookup for TFTP-like independently selected ports by performing a genuinely reversed partial-tuple lookup. |
| !7695 | merged | Scanned | LTE RRC ASN.1 update to v17.1.0; generated/spec maintenance. |
| !7694 | closed | Low / draft | Draft example dissector for extcap_example.py; discussion suggested Lua might be a better example vehicle. Never accepted. |
| !7693 | merged | Scanned | release-3.4 backport of the NAS T3324 timer-type correction from !7691. |
| !7692 | merged | Scanned | release-3.6 backport of the NAS T3324 timer-type correction from !7691. |
| !7691 | merged | Scanned / successor | Accepted master correction for T3324 decoding as GPRS timer 3; clean successor to process-problematic !7677. |
| !7690 | merged | Scanned | L2TP stores Cisco vendor AVP cookie length/session/PW context consistently with IETF AVPs. |
| !7689 | merged | Deep | John Thacker removes a tree-presence guard from substantive L2TP zero-length-body handling: semantic dissection must not depend on whether a protocol tree is being built. |
| !7688 | merged | Deep | John Thacker changes a common, processable Exif-vs-JFIF irregularity from PI_MALFORMED to PI_PROTOCOL, separating structural malformation from protocol-spec deviation. |
| !7687 | merged | Scanned | release-3.6 backport allowing externally reassembled SCCP data; no additional lesson beyond the master behavior. |
| !7686 | merged | Discussion-focused | WLAN keeps the established wlan.fc.type_subtype value representation because changing it would break widespread filters; fixes the field description instead. |
| !7685 | merged | Deep corroboration | Broad conversion from anonymous handles to register_dissector(), explicitly to make dissectors discoverable by find_dissector(), fuzzshark/rawshark, and Lua. |
| !7684 | merged | Deep | John Thacker validates TURN channel identity before trusting a TCP PDU length; invalid/multiplexed data consumes the remaining packet rather than poisoning future stream framing with a bogus length. |
| !7683 | merged | Scanned | STUN/TURN registry and historical comments only. |
| !7682 | merged | Scanned | Names the NFS unknown protocol item more clearly; presentation-only. |
| !7681 | merged | Scanned | KINK default port updated to its final IANA assignment; authoritative registry maintenance. |
| !7680 | merged | Scanned | release-3.4 backport of GSM CBSP repetition-period fix. |
| !7679 | merged | Scanned | release-3.6 backport of GSM CBSP repetition-period fix. |
| !7678 | merged | Deep corroboration | John Thacker moves preference-triggered one-time registration under initialization guards, adopts automatic port/range preferences, and adds legacy preference-name migration mappings. |
| !7677 | closed | Discussion-focused / superseded | Correct NAS T3324 code but submission workflow was wrong: Pascal Quantin distinguished MR title from commit message, flagged use of fork master, and resubmitted cleanly as merged !7691. |
| !7676 | merged | Scanned | Master GSM CBSP repetition-period fix; accepted successor to draft !7675. |
| !7675 | closed | Low / superseded | Draft GSM CBSP repetition-period correction; superseded by Pascal Quantin's merged !7676. |
| !7674 | merged | Scanned | Documentation cleanup for Python naming/HTTPS links. |
| !7673 | merged | Deep | Extcap moves stdout/stderr handling to asynchronous GIOChannel watches and keeps the UI responsive while bounded shutdown is pending. Gerald Combs accepted a bounded disabled-action state rather than UI lockup; Guy Harris clarified Unix-vs-Linux portability language. |
| !7672 | merged | Scanned | Automatic master data/translation update. |
| !7671 | merged | Scanned | Automatic release-3.4 data update. |
| !7670 | merged | Scanned | Automatic release-3.6 data update. |
| !7669 | merged | Scanned | release-3.6 backport of BGP zero next-hop-length guard. |
| !7668 | closed | Negative evidence | John Thacker proposed a leaf null guard; Roland Knall rejected it as a workaround for incorrect underlying model logic. Closed, so retained only as low-weight review guidance. |
| !7667 | merged | Scanned | Further SCTP/port preferences converted to automatic range preferences. |
| !7666 | merged | Scanned | BGP avoids converting a next-hop byte string when declared length is zero; straightforward malformed/edge guard. |
| !7665 | closed | Low / superseded | Release-3.6 BGP fix attempt closed; merged master !7666 and backport !7669 are authoritative. |
| !7664 | merged | Discussion-focused | sshdump adds remote dumpcap/tcpdump selection. Gerald Combs corrected OpenSSH naming; Chuck Craft questioned missing-interface behavior. |
| !7663 | merged | Scanned | Extcap radio-button preference persistence; UI-state fix. |
| !7662 | merged | Scanned | UMTS FP corrects conversation_new wildcard flag (NO_ADDR_B vs NO_PORT2); narrow conversation bug fix. |
| !7661 | merged | Scanned | John Thacker constifies read-only range APIs, strengthening public API const-correctness. |

## Batch-level conclusions

The strongest new material is the extcap lifecycle pair !7673/!7706, especially Guy Harris's cross-platform investigation in !7706; John Thacker's L2TP dispatch/conversation fixes !7702 and !7696; the tree-independence correction !7689; expert taxonomy correction !7688; and TURN stream-framing fix !7684. !7685 and !7678 strongly corroborate later notebook conventions around named dissector registration and one-time registration/preference side effects. Closed MRs !7701 and !7677 are useful submission-workflow evidence only because merged successors establish the accepted technical outcome.
