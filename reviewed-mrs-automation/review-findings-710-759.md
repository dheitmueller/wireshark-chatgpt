# Review findings: Wireshark MRs !710-!759

Reviewed exactly 50 MRs, !759 through !710, at corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054`. Outcomes: 48 merged, !736 open, !723 closed.

Key findings:

- !750/!751: systemd journal data gets a semantic Wiretap record type independent of whether it is stored in a journal export or pcapng.
- !742: use `wtap_uses_interface_ids()` to test the needed file-format capability instead of checking for pcapng by name.
- !730: QUIC Header Protection and Packet Protection state have different lifecycles and are modeled separately so Key Update resets only the state that actually changes.
- !752: reviewer feedback favored structural loop bounds, numeric field types matching the wire width, generated marking for calculated fields, focused captures for review, and a component-prefixed commit subject.
- !736: although still open, maintainer discussion gives useful guidance on source-file organization, symbol visibility, supported interfaces, column handling, packet-lifetime allocation, common helper reuse, display-filter naming, and capture link-layer modeling. Its implementation is not treated as accepted precedent.
- !734/!737/!740: record-count limits are derived from actual frame-number and frontend constraints, with an explicit warning when input records are discarded.
- !748: QUIC Version Negotiation uses an explicit internal state rather than treating unused packet-type bits as meaningful.
- !710/!719/!721: the broad workaround for a shared URL macro was replaced by the root-cause fix, a missing include in one translation unit.
- !747: a floating Fedora CI image changed packaging/CMake behavior when the distro advanced, showing the value of controlled CI environments.
- !757: when a protocol item has unknown final extent, start with length `-1` and shrink it after parsing establishes the endpoint.
- !727: registered field widths should match the actual decoded wire width.
- !713: Pascal Quantin requested enabling maintainer commits on the source branch.
- !712: the stable-branch commit validator accounted for deterministic cherry-pick provenance text.

No ST 291/VANC packet type was encountered.
