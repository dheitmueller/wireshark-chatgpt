# Automated MR review ledger: !10713–!10762

Reviewed using GPT-5.6 Sol.

Corpus repository: \`dheitmueller/wireshark-corpus-mrs\`

Corpus commit: \`ddcaa22b51c68f594e425a23388c3a2086813054\`

Exactly 50 previously unreviewed merge requests were reviewed in this run. Selection was made from the exact already-reviewed MR set reconstructed from \`reviewed-mrs.md\` and all available \`reviewed-mrs-automation/\` tracking on notebook \`main\`; range filenames were not treated as proof of coverage. The historical !17571–!17620 ledger was explicitly verified to contain all 50 MR numbers. Merged MRs are treated as stronger evidence than closed/superseded work, and authoritative maintainer feedback is weighted accordingly.

| MR | Outcome | Depth | Notes |
|---|---|---|---|
| !10762 | merged | Deep / promoted | John Thacker moves MySQL's interpretation state from conversation-only "latest" values into per-PDU snapshots for random-access redissection; first-pass mutations are visited-guarded. Promoted to \`dissector-state-conventions.md\`. |
| !10761 | merged | Discussion-focused | RPM setup dependency fix; John Thacker supplied the SUSE package-name counterpart. Packaging-specific. |
| !10760 | merged | Scanned | Missing include fix for no-libpcap builds; portability maintenance, no new general rule. |
| !10759 | merged | Scanned / high-authority acceptance | Case-sensitive-filesystem build fixes; merged by Guy Harris. Useful portability evidence but no distinct new convention. |
| !10758 | merged | Scanned | Windows/CMake architecture naming cleanup from win64arm to arm64. |
| !10757 | merged | Scanned | CI/documentation follow-up for win64-to-x64 naming. |
| !10756 | merged | Deep / promoted | XRA parser used overly narrow integer storage for offsets/derived lengths and could wrap into an infinite loop. Promoted to \`parser-progress-conventions.md\`. |
| !10755 | closed draft | Discussion-focused / down-weighted | Arkime shortheader-pcap proposal. Guy Harris strongly favored its distinct magic number over heuristic pcap discrimination and suggested libpcap as the right upstream layer; author closed because no sample file was available. Context only because unmerged. |
| !10754 | merged | Scanned | GitLab CI Windows package-glob update. |
| !10753 | merged | Discussion-focused | MySQL 0xfb response handling; John Thacker noted pipelining can legitimately leave state at REQUEST, so response classification must account for that. Protocol-specific. |
| !10752 | merged | Scanned | Moves MSYS2 instructions into the developer guide. |
| !10751 | merged | Discussion-focused | CMake Npcap/USBPcap download refactor; review covered single-source dependency management, persistent caches, and offline builds. Build-system-specific. |
| !10750 | merged | Scanned | Adds Python cache files to gitignore. |
| !10749 | merged | Scanned | RDP DRDYNVC typo/copy-paste cleanup. |
| !10748 | merged | Discussion-focused | Email/mailmap update; review clarified AUTHORS and .mailmap interaction. Repository-maintenance-specific. |
| !10747 | merged | Deep / promoted | John Thacker fixes PER fragmented bit/octet string leaks by running the potentially throwing fragment constructor before creating an unattached composite tvbuff. Promoted with !10728 to \`memory-lifetime-conventions.md\`. |
| !10746 | merged | Scanned | RTPS secure-prefix flag additions; protocol-specific. |
| !10745 | closed / superseded | Discussion-focused / down-weighted | Thread 1.2/1.3 submission. Review requested pcaps, warning cleanup, accurate title, topic-branch workflow, and maintainer-edit permission; author closed to resubmit as !11008. Corroborates existing submission guidance only. |
| !10744 | merged | Scanned | Windows installer filename-format change. |
| !10743 | merged | Scanned / corroborating | XMPP exception cleanup fixes fuzz-found leaks; reinforces existing exception-safe cleanup guidance. |
| !10742 | merged | Scanned | Release preparation for 3.6.14. |
| !10741 | merged | Scanned | Release preparation for 4.0.6. |
| !10740 | merged | Discussion-focused | extcap UI fix; Tomasz Moń recommended durable commit-hash provenance in Fixes references rather than relying on an MR number. |
| !10739 | merged | Deep / review-focused | John Thacker read the BMP RFC's "set of TLVs" literally and caught a single-item parser; accepted code iterates TLVs to the PDU boundary. Strong specification-cardinality review evidence, no duplicate notebook rule added. |
| !10738 | merged | Discussion-focused | Clang analyzer dead-initialization cleanup; review distinguished a dead initialization from a live later value. Static-analysis corroboration. |
| !10737 | merged | Scanned | batadv uninitialized-variable stable backport. |
| !10736 | merged | Scanned | batadv uninitialized-variable stable backport. |
| !10735 | merged | Scanned | batadv uninitialized-variable master fix. |
| !10734 | merged | Discussion-focused | AirPcap disabled for MSYS2 because the SDK is unavailable there; Guy Harris requested documenting that causal platform constraint in CMake. |
| !10733 | merged | Discussion-focused | New UDP BitTorrent Tracker dissector; contributor supplied a pcap, added release notes, and fixed checker warning after Alexis La Goutte review. Corroborates existing submission/test practice. |
| !10732 | merged | Deep / promoted | Decode As now distinguishes explicit no binding from reset-to-default; NULL becomes a real current state rather than an overloaded reset sentinel. Promoted to \`state-representation-conventions.md\`. |
| !10731 | merged | Scanned / corroborating | DOF replaces GSList with a file-scoped wmem list so container nodes share the owning lifetime. Existing allocator/container guidance covers it. |
| !10730 | merged | Scanned | USB HID label typo stable backport. |
| !10729 | merged | Scanned | USB HID label typo stable backport. |
| !10728 | merged | Deep / promoted | John Thacker fixes PER open-type exception leak with the same acquisition-before-unattached-owner ordering as !10747. Promoted jointly to \`memory-lifetime-conventions.md\`. |
| !10727 | merged | Deep / corroborating | Decode As encapsulates handle + description and changes them together, supporting the stronger explicit-state rule from !10732. |
| !10726 | merged | Scanned | USB HID label typo master fix. |
| !10725 | merged | Deep / corroborating | John Thacker fixes multiple SNMP PDUs over TCP and desegmentation return semantics. Jaap Keuter caught direct edits to generated packet-snmp.c and required the ASN.1 template source be changed; reinforces existing generated-code source-of-truth guidance. |
| !10724 | closed | Scanned / down-weighted | Oversized Thread/x64 submission with extensive unrelated churn; closed unmerged, no accepted implementation lesson. |
| !10723 | merged | Deep / promoted | Reassembly-table replacement must preserve old tvbuff backing bytes when addresses may still alias them via set_address_tvb(); old tvb is chained to the replacement. Promoted to \`memory-lifetime-conventions.md\`. |
| !10722 | merged | Scanned | Removes bundled x64.nsh in favor of NSIS's copy. |
| !10721 | merged | Scanned | Extends check_tfs.py and fixes findings; static-check maintenance. |
| !10720 | merged | Scanned | Updates Windows libssh and adds Arm64 Lua packages. |
| !10719 | merged | Scanned | Automatic data/translation update. |
| !10718 | merged | Scanned | Automatic data update for a stable branch. |
| !10717 | merged | Scanned | Automatic data update for a stable branch. |
| !10716 | closed draft | Discussion-focused / down-weighted | Qt column-header/redissection experiment exposed stale file-scope/conversation state, selection restoration, and sorting-order hazards. Author considered the design unsatisfactory and closed it; retain only as negative/contextual evidence. |
| !10715 | merged | Scanned | Initializes variables to satisfy maybe-uninitialized diagnostics. |
| !10714 | merged | Deep / corroborating | Synphasor stable fix replaces packet-controlled direct indexing into an incomplete value_string with safe lookup + Unknown fallback. The master counterpart lies below this batch, so promotion is deferred until the master MR is reviewed. |
| !10713 | merged | Deep / corroborating | Second stable counterpart of the Synphasor safe value_string lookup fix; same lesson as !10714, with master review intentionally deferred. |

## Durable findings promoted

- !10762: first-pass state machines should snapshot the interpretation state at each semantic PDU boundary and use those snapshots for random access instead of replaying mutable conversation state.
- !10728 and !10747: if a prerequisite operation can throw, perform it before creating an object that is not yet attached to an automatic ownership chain; audit ownership after every acquisition step, not only at the final structure.
- !10723: replacing a table entry does not end the lifetime of aliases into its tvbuff bytes; transfer/chaining ownership must preserve backing storage until all aliases are done.
- !10756: parser cursor width follows the addressable input/arithmetic domain, not the width of the wire field that originally supplied a value.
- !10732 (with !10727): "default" and explicit "none" need distinct state representations when they have different dispatch semantics.

## Strong corroboration retained without duplicate rules

- !10725 explicitly reinforces generated-source discipline: edit the ASN.1 template/canonical source and regenerate the generated dissector, rather than patching generated output alone.
- !10739 shows protocol-specification cardinality is a parser invariant: a "set/list of TLVs" must be implemented as a repeated structure bounded by the containing PDU.
- !10733 and closed !10745 reinforce representative-capture and clean-checker expectations for dissector submissions.
- !10743 reinforces exception-safe cleanup under fuzz-triggered errors.
- !10755 supplies high-authority but unmerged context from Guy Harris that distinct file-format magic is preferable to heuristic discrimination; it was not promoted as an accepted architecture rule.

## Exact reviewed MR set

!10762 !10761 !10760 !10759 !10758 !10757 !10756 !10755 !10754 !10753 !10752 !10751 !10750 !10749 !10748 !10747 !10746 !10745 !10744 !10743 !10742 !10741 !10740 !10739 !10738 !10737 !10736 !10735 !10734 !10733 !10732 !10731 !10730 !10729 !10728 !10727 !10726 !10725 !10724 !10723 !10722 !10721 !10720 !10719 !10718 !10717 !10716 !10715 !10714 !10713
