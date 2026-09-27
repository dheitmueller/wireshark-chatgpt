# Review findings: Wireshark MRs !7961-!8010

Corpus: `dheitmueller/wireshark-corpus-mrs@ddcaa22b51c68f594e425a23388c3a2086813054`

Merged master work and substantive maintainer review are strongest. Release backports are corroborating; closed !7999 is negative/submission-process evidence only.

| MR | Outcome | Finding |
|---|---|---|
| !8010 | merged master; Guy Harris | Renames AppleTalk/DSI handoff fields to their real shared semantics: ATP transaction / DSI request ID, not an ASP sequence number. |
| !8009 | merged release-4.0 | Backport of !8008 expert-info behavior. |
| !8008 | merged master; Guy Harris | Tree-reference optimization must not suppress expert information; expert output has consumers independent of protocol-tree visibility. |
| !8007 | merged release-4.0 | Backport of !8005 TVBuff invariant repair. |
| !8006 | merged master | ROHC comment cleanup; no durable new rule. |
| !8005 | merged master; Guy Harris | A TVBuff with reported length below captured length is internally inconsistent; diagnose and repair it before downstream dissection. |
| !8004 | merged release-4.0 | AT+CGDCONT backport; no additional lesson. |
| !8003 | merged master; John Thacker | Normalize negative TVBuff search offsets/limits once at API entry and use one bounded internal coordinate system. |
| !8002 | merged release-4.0 | Backport removing stray LAPDm debug output. |
| !8001 | merged master | Removes stray debug output; pre-submit hygiene only. |
| !8000 | merged master | manuf.tmpl may intentionally override imported IEEE OUI naming, but the replacement should still match the registry's formal company name. |
| !7999 | closed | MiWi submission had checker failures, prohibited API use, filter-prefix mismatches, and a conflicted unsquashed branch. Negative submission evidence only. |
| !7998 | merged master | AT+CGDCONT support; no reusable review lesson. |
| !7997 | merged release-4.0 | Backport restoring public conversation helper and symbol entry. |
| !7996 | merged master; Guy Harris | Restores a public helper because third-party code may use it. Public-API removals must consider out-of-tree consumers and symbol manifests. |
| !7995 | merged master | Logray resource changes update macOS, Windows, NSIS, Qt, and docs together; resource changes must be traced across packaging surfaces. |
| !7994 | merged master | UDS standards-name data update. |
| !7993 | merged release-4.0 | Backport of unsigned AccECN counter fix. |
| !7992 | merged release-4.0 | TCP expert info for non-zero bytes after EOL; anomaly visibility. |
| !7991 | merged release-4.0 | SACK-permitted presentation cleanup. |
| !7990 | merged release-4.0 | AccECN support backport. |
| !7989 | merged release-4.0 | TARR option support backport. |
| !7988 | merged release-4.0 | Uses RFC 6994 ExID semantics, preserves the old preference name through migration, and adds a focused capture. |
| !7987 | merged master | BGP registry update. |
| !7986 | merged release-4.0 | Backport fixing optional conversation debug code. |
| !7985 | merged master; Guy Harris | Optional/debug code had bit-rotted; useful reminder that debug build paths need compilation coverage. |
| !7984 | merged release-4.0 | Backport of !7982 map-key lifetime fix. |
| !7983 | merged master | Wireshark/Logray CMake separation; project-specific cleanup. |
| !7982 | merged master; John Thacker + Guy Harris | Persistent HTTP map must not keep an address into stack-local tcpinfo. Final key directly encodes the unsigned sequence value. |
| !7981 | merged master | F5 trailer loop now rejects invalid length/type and exits if a child consumes zero bytes; every recoverable iteration must make progress. |
| !7980 | merged master; John Thacker | TCP analysis labels and reassembly need are not identical; first-pass state records data that can safely be ignored on revisits. |
| !7979 | merged master | BACnet fixes; no broader convention. |
| !7978 | merged master | Review caught a static-analysis dead store in S7comm; warnings are actionable review feedback. |
| !7977 | merged release-4.0; Gerald Combs | ABI CI uses release-specific dumps, runs all library checks, preserves each result, and publishes reports before failing. |
| !7976 | merged release-3.6; Gerald Combs | Toolchain changes caused ABI-check noise; fix controls DWARF/CMake flags instead of accepting spurious failures. |
| !7975 | merged master | MKA field-name semantic clarification. |
| !7974 | merged release-4.0 | Backport of conversation-identity documentation from !7970. |
| !7973 | merged master | Restores extcap termination behavior accidentally reverted earlier. |
| !7972 | merged master | Windows build exposed missing direct stdlib include for strtol; do not rely on transitive standard-library includes. |
| !7971 | merged master | TEAP sub-TLV support; no reusable review lesson. |
| !7970 | merged master; Guy Harris | Documents distinct arbitrary-element and address/port conversation identity mechanisms and why endpoint overrides exist. |
| !7969 | merged master | AccECN 24-bit counters must be unsigned; ordinary semantic-range fix. |
| !7968 | merged release-4.0 | Backport of !7966 conversation terminology change. |
| !7967 | merged master; John Thacker | Percent decoder gains explicit pointer+length input for arbitrary bytes, including internal NULs, with compatibility fallback for older GLib. |
| !7966 | merged master; Guy Harris | Renames generic-sounding conversation-key state to address/port endpoints because arbitrary conversation identity uses element arrays. |
| !7965 | merged release-4.0 | Strict TCP OOO comparison avoids adding a zero-length segment; progress corroboration. |
| !7964 | merged release-4.0 | Diameter dictionary-fix backport. |
| !7963 | merged master | Diameter dictionary copy/paste fixes. |
| !7962 | merged release-4.0 | OCP.1 value-string tables made static. |
| !7961 | merged release-4.0 | OCP.1 rejects PDU sizes below the structural header minimum before loop advancement and in heuristic recognition. |

## Promoted durable rules

- TVBuff captured/reported-length invariants and normalized helper coordinates: !8005, !8003.
- Protocol-tree optimization must preserve expert-information side effects: !8008/!8009.
- Persistent map keys must own or directly encode identity, never point into ephemeral packet stack storage: !7982/!7984.
- Conversation identity has distinct address/port and arbitrary-element models; APIs should name the actual model: !7970/!7974, !7966/!7968.
- Public API compatibility includes third-party callers and symbol manifests; ABI CI needs release-specific baselines and complete diagnostics: !7996/!7997, !7977/!7976.
- Variable-length parser loops must prove positive progress: !7981, !7965, !7961.
- Byte-oriented decoding helpers should accept explicit lengths rather than assuming NUL termination when packet selections can contain arbitrary bytes: !7967.
