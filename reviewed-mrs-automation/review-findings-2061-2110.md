# Review findings — !2061–!2110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Exactly 50 MRs were reviewed, descending !2110 through !2061. Outcomes: 47 merged; closed/unmerged !2091, !2073, and !2063. Merged work was weighted above closed work, and direct maintainer guidance was weighted according to authority.

Strongest accepted evidence:
- !2092 (Guy Harris): move built-in Wiretap file-type registration into the owning format modules and use runtime registration rather than a central fixed table.
- !2080, !2082, !2076, !2088 (Guy Harris): distinguish stable machine file-format names from human descriptions; machine names should be suitable for lookup/CLI use, and new Lua code should use semantic name lookup rather than the compatibility numeric table.
- !2103 with Pascal Quantin review: expert-analysis inputs must not disappear merely because no visible protocol tree was requested; audit nearby expert paths for the same tree-gating mistake.
- !2096 with Anders Broman review: replacement dependency APIs must remain compatible with Wireshark's declared minimum dependency version.
- !2104 (Gerald Combs): use target-scoped CMake include requirements and mark external dependency headers as SYSTEM at the target that consumes them.
- !2074 (Gerald Combs): treat packet-derived external content as a trust boundary; use conservative preview behavior and inspect actual content before invoking a desktop viewer.
- !2081: propagate lower-layer error-packet state so nested dissectors do not overwrite the intended error context.
- !2066: use Wireshark's standard range-string machinery when a numeric protocol registry is naturally expressed as ranges.

Qualified evidence:
- !2072 merged, but the author later supplied a capture demonstrating a remaining failure and suggested reverting pending a more accurate TCP ordering rule. Treat it as a testing/review caution, not a preferred heuristic.
- Closed !2073 contains useful João Valverde guidance against a wrapper dissector that adds no semantics beyond XML/SOAP dispatch; use the generic dispatch/Decode-As mechanism where it already models the protocol.
- Closed !2063 contains high-authority lifecycle discussion favoring a common explicit extcap shutdown mechanism and nonblocking event-loop integration over platform-specific process-control tricks, but the proposed implementation did not merge.
- !2091 contained no substantive changes and carries no engineering weight.

The remaining MRs were scanned for purpose, outcome, changed files, and discussion. They were routine backports, automatic data updates, documentation changes, straightforward protocol additions/fixes, or local linkage/formatting cleanups that did not add a new durable convention beyond the notebook's existing guidance.

No SMPTE ST 291/VANC packet type was encountered.
