# Reviewed Wireshark MRs — automation batch !15449–!15498

Corpus repository: `dheitmueller/wireshark-corpus-mrs`

Corpus commit: `130eea3c8a60902c0fc98c9e1130e73bafc6f815`

Selection method: rebuilt the already-reviewed set from the available notebook review tracking, including `reviewed-mrs.md`, the aggregate automation tracking, and the per-run ledgers under `reviewed-mrs-automation/`. The historical !17571–!17620 batch is explicitly preserved and counted. Selection is by exact MR-number set subtraction against the corpus, not by assuming that a numeric interval is covered because some members appear in a ledger. No prior tracking entry covered any MR in this batch, so the fifty highest-numbered available unreviewed MRs are the contiguous set !15498 through !15449.

## Exact reviewed MR set

- !15498, !15497, !15496, !15495, !15494, !15493, !15492, !15491, !15490, !15489
- !15488, !15487, !15486, !15485, !15484, !15483, !15482, !15481, !15480, !15479
- !15478, !15477, !15476, !15475, !15474, !15473, !15472, !15471, !15470, !15469
- !15468, !15467, !15466, !15465, !15464, !15463, !15462, !15461, !15460, !15459
- !15458, !15457, !15456, !15455, !15454, !15453, !15452, !15451, !15450, !15449

Count: **50**.

Merge-status weighting: **48 merged**, **2 closed/unmerged** (!15475 and !15466). Closed proposals were inspected but down-weighted. Merged !15464 was also down-weighted as final architectural evidence because its MySQL interpretation was subsequently superseded by the merged !15539 revert/correction.

## Review notes and weighting

- **!15491 — deep, merged master; promoted.** `PFC_HDR_SIGNING` is negotiated once per DCE/RPC transport connection and applies across authentication contexts. The accepted change moves that flag into transport-level state keyed by conversation plus transport salt. John Thacker reviewed the map/hash implementation and merged the MR. Added `conversation-state-scope-conventions.md`: persistent state must be owned and keyed at the semantic scope defined by the protocol rather than whichever narrower object first observes it.
- **!15485 + !15497 — deep, merged master plus stable backport; promoted.** John Thacker fixes LAPD by populating `pinfo->dl_src`/`dl_dst` and effective `src`/`dst` addresses instead of manually maintaining only resolved columns. This fixes Flow Graph and makes resolved/unresolved presentation share one semantic source of truth. Added `packet-address-conventions.md`.
- **!15477 — deep, merged master; high weight.** Authored and merged by John Thacker. DTLS 1.3 unified headers no longer expose the strong cleartext discriminators used by the older heuristic, so the accepted heuristic requires an already-established DTLS session and matching CID where applicable rather than broadening stateless recognition. Added a state-backed recognition rule to `heuristic-dissector-conventions.md`.
- **!15474 — deep, merged master; high weight.** Gerald Combs converts the Wireshark-owned preferences API from GLib scalar types to standard C types, including `guint32`→`uint32_t` and `gboolean`→`bool`; Anders Broman approved and merged it. Extended `c-type-conventions.md` so the standard-C-type rule explicitly covers Wireshark-owned APIs as well as dissector-local code while preserving dependency-specific types at true external boundaries.
- **!15469 + !15468 — deep, merged master; promoted.** John Thacker uses `const` to encode two architectural contracts: registered `header_field_info` metadata is shared/read-only through `field_info`, and conversation lookup functions only inspect `packet_info` and should therefore compose with callbacks that receive `const packet_info *`. Added `const-correctness-conventions.md`.
- **!15464 — deep, merged but superseded; high-authority cautionary evidence.** The original MySQL multibyte-collation change generalized an observed two-byte Connector/Python behavior. Guy Harris explicitly challenged the interpretation with evidence that the server reads one byte as documented and that the two-byte sender was an unfixed Connector/Python bug. The later merged !15539 reverted/corrected the broad interpretation. Extended `protocol-implementation-compatibility-conventions.md` to record !15464 as a precursor showing why implementation-specific observations need sender/receiver/specification cross-checking, while deliberately not treating its merged code as final policy.
- **!15466 — closed draft; down-weighted, but useful policy corroboration.** The proposal attempted to backport sharkd microsecond I/O Graph intervals to release-4.2. John Thacker distinguished possible bug-fix backports from the microsecond-interval enhancement and stated that the enhancement was not eligible for 4.2. This corroborates the existing stable-branch rule that feature intake belongs on master; no duplicate notebook rule was added.
- **!15475 — closed/unmerged; down-weighted.** First-contribution Apple BT LE extensions proposal with substantial iteration and a failed final pipeline. It was not treated as accepted architectural guidance.
- **!15473 — deep, merged master; parser-bounds corroboration.** John Thacker fixes DTLS ServerHello scanning that mixed an outer record length with the inner 12-byte handshake-header offset, producing an oversized end boundary. The accepted code uses the handshake fragment length and validates fragment availability before scanning. This reinforces existing tvbuff/parser boundary guidance; no duplicate rule was added.
- **!15460 — deep, merged master; error-contract corroboration.** John Thacker fixes a Coverity-found NULL out-parameter bug by validating `key_bin` with `ws_return_val_if()` before any dereference, distinguishing a caller programming error from a NULL input string that can be reported through a valid out parameter. Useful evidence for checking output-pointer preconditions before writing through them; no separate file was needed in this run.
- **!15455 — merged master; contextual-state evidence.** Bluetooth LE dissection uses stored periodic-advertising context to recover useful dissection when the raw PDU type is unavailable. The MR itself notes that some state types still needed cleanup, and later BTLE work in !15609/!15642 refined state typing. It was therefore retained as corroboration rather than promoted independently.
- **!15450 — merged stable backport; warning-policy evidence.** John Thacker suppresses c-ares deprecation warnings only for dependency versions that emit them because deprecated APIs can remain a deliberate runtime fallback when thread support is unavailable. This is a scoped, documented suppression tied to a real supported configuration rather than an attempt to hide an actionable compiler diagnostic; retained as supporting evidence.
- **!15449 — merged stable backport; strong corroboration.** John Thacker fixes WSLua signed integer validation so a possibly negative `Lua_Number` is checked/converted directly as signed instead of passing through unsigned conversion, whose behavior differs across architectures. This strongly corroborates the existing scripting numeric-conversion conventions without duplicating them.
- **!15487 — merged master; semantic/UI corroboration.** John Thacker makes the sequence-analysis `any_addr` flag mean what its name and UI label say and stores explicit semantic values in combo-box user data rather than depending on contradictory index/default behavior. The same correction fixes tshark's flow tap, reinforcing that shared model semantics must not be defined solely by one UI presentation.
- **!15492, !15490, !15470, !15463/!15462/!15461/!15456, and other protocol-specific MRs — reviewed.** These provide useful DCE/RPC, GSS-API, 5G NR, PTP, and other dissector corrections, but their accepted changes were protocol-local or already covered by broader notebook guidance.
- **!15495/!15494/!15489/!15488/!15486/!15480/!15478/!15467/!15458 and related Qt work — reviewed.** Mostly Flow Graph placement/layout, tooltips, and presentation cleanup. Useful implementation evidence but no additional durable cross-project rule beyond existing UI conventions.
- **!15493/!15483/!15481/!15479/!15472/!15471/!15465/!15459/!15457/!15451 and related build/CI changes — reviewed.** These are focused Qt discovery, GitHub/GitLab action, Logray, CMake, falcodump, and documentation-job maintenance. No new project-wide convention was promoted from them.
- **!15476/!15454/!15453/!15452 and remaining narrow fixes — reviewed.** Buffer-size/truncation and generated-data or protocol-table maintenance were accepted but did not add a durable rule beyond existing bounds/type/tooling guidance.

## Notebook updates from this run

- `conversation-state-scope-conventions.md`: new rule that persistent negotiated state belongs at the protocol scope that owns it and must be keyed by the complete semantic connection identity (!15491).
- `packet-address-conventions.md`: new rule to populate semantic `packet_info` address fields and let columns/graphs/taps consume them instead of manually synchronizing presentation strings (!15485, !15497).
- `const-correctness-conventions.md`: new ownership/query rules for read-only shared metadata and read-only lookup context (!15469, !15468).
- `heuristic-dissector-conventions.md`: added established-session state as a legitimate way to retain selectivity when encrypted/compact headers remove clear discriminators (!15477).
- `c-type-conventions.md`: broadened the standard-C-type convention from dissector-local code to Wireshark-owned APIs, with dependency-boundary exceptions preserved (!15474).
- `protocol-implementation-compatibility-conventions.md`: added direct Guy Harris review of superseded !15464 as high-authority evidence for cross-checking implementation quirks against the specification and receiver behavior before generalizing wire semantics.

## Continuation

`mr_15448.json` exists at the same corpus commit, so the corpus is not exhausted and the next run can continue below this batch.
