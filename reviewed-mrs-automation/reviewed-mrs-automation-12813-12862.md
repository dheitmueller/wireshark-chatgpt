# Automated Wireshark MR review: !12813-!12862

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Review direction: descending from the newest available previously-unreviewed MR.

Selection was rebuilt from the available explicit review tracking in `reviewed-mrs.md`, the supplemental automation tracker, and the per-run ledgers under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains counted. The prior !12862 lookup was only a frontier check and was not counted as reviewed. No tracking entry marked any member of this candidate set reviewed, so this run reviews exactly these 50 MRs:

`!12862, !12861, !12860, !12859, !12858, !12857, !12856, !12855, !12854, !12853, !12852, !12851, !12850, !12849, !12848, !12847, !12846, !12845, !12844, !12843, !12842, !12841, !12840, !12839, !12838, !12837, !12836, !12835, !12834, !12833, !12832, !12831, !12830, !12829, !12828, !12827, !12826, !12825, !12824, !12823, !12822, !12821, !12820, !12819, !12818, !12817, !12816, !12815, !12814, !12813`

Status weighting: 49 records are merged. !12844 is a closed, unmerged draft and is therefore deliberately lower-weight than the accepted work around it.

## Durable findings

- **!12840 + !12861 — deep, merged master, very high weight.** `FT_IPv4` was moved away from generic unsigned-integer fvalue accessors to dedicated IPv4 APIs. John Thacker's !12861 then exposed why that matters: the Qt address editor mixed the representation returned by `fvalue_get_ipv4()` with the representation expected by `set_address()`, reversing dotted-quad display. The accepted path uses the typed IPv4 object/address storage and makes the byte-order boundary explicit. Added `address-representation-conventions.md`. Closed draft !12844 is retained only as lower-weight evidence that the broader IPv4 integer/byte-order model was known to be confusing.

- **!12851 — deep, merged master, extremely high weight.** John Thacker fixes TCP sequence-analysis override preference handling by staging the target frames while the dialog is open and applying changes only during unstash/accept. The old path mutated `frame_data` immediately, so Reject still changed behavior and left state inconsistent because redissection was not triggered. The accepted path also marks effects only for real changes, avoiding needless full redissection. Added `preference-edit-transaction-conventions.md`.

- **!12834 — deep, merged master, very high weight.** NR-RRC needed UE identity when captures begin at RLC/PDCP instead of MAC. Martin Mathieson warned that simply replacing MAC lookup would break existing DCT2000/private capture paths and proposed a common ordered lookup helper. Pascal Quantin explicitly required preserving existing behavior. The accepted implementation checks MAC, then RLC, then PDCP, and the MR carries example MAC/RLC captures. Added `protocol-context-fallback-conventions.md`. Martin's explanation that ASN.1 changes belong in `.cnf`/template inputs and are regenerated strongly corroborates the existing `generated-code-conventions.md` rule.

- **!12820 — substantive, merged master.** Clang's compiler-compatibility macros do not imply full support for the other compiler's pragma surface, and clang-cl does not behave like ordinary Clang/GCC macro selection. The accepted attribute logic tests `__clang__` explicitly and routes Clang through `__attribute__`. Added `compiler-feature-selection-conventions.md`.

- **!12836 — deep/corroborating, merged master, John Thacker-authored.** RTP now distinguishes genuinely invalid padding (zero or larger than the payload) from capture truncation that merely prevents decoding/validation. The latter is not `PI_MALFORMED/PI_ERROR`. This strongly corroborates the existing `validation-availability-conventions.md` distinction between invalid input and unavailable validation, so no duplicate rule was added.

- **!12846 — deep/corroborating, merged master.** Gilbert Ramirez's review of Lua `DissectorTable` GUID support repeatedly pushes toward exact comparisons, explanatory comments for non-obvious conditions, file-local `static` helpers, simpler wmem APIs where possible, and—most notably—factoring duplicated tricky DCE/RPC registration steps into a shared helper so future fixes do not drift between copies. The accepted result implements the review and Gilbert approves it. The duplication lesson is already covered by `refactoring-conventions.md`, so it is recorded here without another rule.

- **!12843 / !12852 — substantive merged master/backport pair.** HTTP/3 moves QPACK allocation after an assertion and deallocation before another early return to close error-path leaks, adds control-stream frame dissection, fixes displayed frame spans, and places frame payload under the corresponding frame item. This is useful error-path ownership and tree-structure evidence already covered by existing lifetime/tree guidance.

- **!12848 / !12849 / !12850 — merged master plus release backports.** The preferences UI's temporary `prefSearchData` exists only for a synchronous foreach call, so the accepted master fix uses stack lifetime instead of leaking a heap allocation. Guy Harris approved the master change and authored the release backports, giving the simple lifetime correction strong authority without needing a new notebook rule.

- **!12837 / !12842 — merged master/backport.** Qt 6.6 deprecates `qAsConst`, but John Thacker's review checks the interaction with Qt 5/C++ standard support and implicit-container detach semantics rather than blindly replacing it with `std::as_const`. Useful compatibility evidence, but too framework-specific for a new durable rule.

- **!12823 — merged master, John Thacker-authored.** The documentation build graph now models the real dependency (`docs` depends on `manpages`) instead of making two targets independently depend on the same generated files, satisfying Xcode's new build system. This corroborates existing build-graph dependency guidance.

- **!12815 — substantive, merged master.** OPC UA decryption secrets become pcapng DSB data that `editcap` can embed and the plugin can consume; the change includes parser sanity checking, MSVC portability work, and an encrypted sample capture with embedded keys. Useful cross-component/plugin and capture-backed test evidence, but no stronger new general convention was needed.

- **!12821 / !12835 — merged master/backport.** TLS surfaces high-value SNI and selected common-extension details while explicitly suppressing GREASE values that should not affect handshake interpretation. This is a usability/display refinement, not a new architecture rule.

- **!12833 — merged.** BLF metadata records use the file's start offset rather than zero so relative-time display matches Vector tooling. Correctness fix, but narrow.

- **!12832 — merged.** OPC UA avoids parsing a service payload when the message form has no payload, fixing a failure identified in earlier work. Corroborates guarding optional parser bodies by the message's actual semantic shape.

- **!12829 — merged.** UDS subfunction dissection masks out `SuppressPositiveResponse` before interpreting the subfunction, with an additional Clang-driven local-scope cleanup. Straightforward field-mask correctness.

- **!12818 — merged, Martin Mathieson-authored.** A cluster of typed-item/checker fixes brings dissector declarations/calls back into checker conformance. It reinforces the notebook's existing typed-item validation/checker practice rather than creating a new rule.

The remaining MRs are routine release backports, CI/version-file plumbing, UI wording/menu changes, straightforward protocol flag/value fixes, or small lifetime/static-scope cleanups. They were individually counted and inspected but did not add stronger durable guidance than the existing notebook.

## Notebook updates

- Added `address-representation-conventions.md` from merged !12840/!12861; closed !12844 is noted only as secondary motivation.
- Added `preference-edit-transaction-conventions.md` from John Thacker's merged !12851.
- Added `protocol-context-fallback-conventions.md` from merged !12834 and its Martin Mathieson/Pascal Quantin review.
- Added `compiler-feature-selection-conventions.md` from merged !12820.
- Retained !12836 as corroboration of `validation-availability-conventions.md`, !12834 as corroboration of `generated-code-conventions.md`, and !12846 as corroboration of `refactoring-conventions.md` rather than duplicating those rules.

## Frontier

`!12812` exists in the corpus (`OER: Support signed field for unconstrained INTEGER`) and is merged. It was inspected only as the next descending frontier candidate and is **not** counted as reviewed by this ledger.
