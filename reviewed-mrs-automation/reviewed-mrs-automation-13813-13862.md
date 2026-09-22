# Wireshark MR review run: !13862 through !13813

Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`

Notebook starting commit: `a02000b39cd4f2051515017583d4ead2ba3a21e7`

Selection was reconciled against the available review tracking in `reviewed-mrs.md`, the aggregate automation tracking, and the per-run ledger inventory under `reviewed-mrs-automation/`. The historical !17571-!17620 batch remains part of the already-reviewed set. The preceding exact ledger covered !13912 through !13863 and explicitly treated !13862 only as a frontier probe, not as reviewed. The corpus remained at the same commit before and after this review, so exact set subtraction selected the fifty highest-numbered previously unreviewed corpus MRs listed below.

## Exact reviewed set

!13862, !13861, !13860, !13859, !13858, !13857, !13856, !13855, !13854, !13853,
!13852, !13851, !13850, !13849, !13848, !13847, !13846, !13845, !13844, !13843,
!13842, !13841, !13840, !13839, !13838, !13837, !13836, !13835, !13834, !13833,
!13832, !13831, !13830, !13829, !13828, !13827, !13826, !13825, !13824, !13823,
!13822, !13821, !13820, !13819, !13818, !13817, !13816, !13815, !13814, !13813.

Exactly 50 MRs were reviewed. Status mix: **49 merged; one closed/unmerged (!13815)**. !13815 proposed a narrow USB class/protocol fallback and was superseded by the more general merged !13825 approach, so the closed proposal was treated as useful negative/review evidence but given substantially less architectural weight than the accepted implementation.

## Durable findings promoted

- **!13860 — private helper pipes must not leak into unrelated exec'd children.** John Thacker's merged master fix explains a MaxMind helper shutdown hang: a later `dumpcap` fork/exec inherited `mmdbresolve` pipe descriptors, keeping the pipe alive after Wireshark closed its own endpoint and leaving Wireshark blocked in `waitpid()`. `subprocess-ipc-lifecycle-conventions.md` now records that private helper descriptors should be close-on-exec unless inheritance is explicitly part of the child protocol, and that pipe-lifetime debugging must account for every inherited duplicate rather than only the conceptual parent/helper endpoints.

- **!13844 and !13846 — child logging is not a command-failure record.** John Thacker's merged changes separate `SP_LOG_MSG` from `SP_ERROR_MSG`, preserve log level, let interface/statistics loops consume intervening log records while continuing toward the real terminal result, and propagate the parent's effective Capchild logging policy to `dumpcap`. `subprocess-ipc-lifecycle-conventions.md` records the broader IPC rule: observability records and control/result records need distinct semantics even when they share one transport.

- **!13853 — generic helpers must distinguish unsupported semantic domains from impossible programmer states.** John Thacker changes service lookup for nonstandard port types from an assertion crash to the ordinary “no service name” result because generic callers can legitimately carry a broader port-type domain than the resolver supports. Evan Huus caught that the fallback also had to preserve the optional `value_ret` out-parameter contract, and John explicitly added an enum unknown value rather than storing an arbitrary out-of-domain integer. These rules were added to `assertion-static-analysis-conventions.md`.

- **!13828, with !13829/!13830/!13831 stable backports — classify diagnostics by failure ownership before telling users where to report them.** This master change was authored and merged by Guy Harris and therefore receives particularly high weight. Recognized remote-capture connection failures should point toward the remote/network environment rather than be wrapped in generic “report this as a Wireshark bug” guidance. `api-error-reporting-conventions.md` now records the ownership-classification rule and notes that repeated parsing of dependency error strings indicates a lower-level API lacking structured error classes.

- **!13845 — recursion guards must charge the budget for real generated-call expansion.** Gerald Combs's merged `asn2wrs` hardening initially guarded only a representative frame in a recursive cycle. Evan Huus pointed out that a long mutually recursive cycle could consume many stack frames per apparent depth increment. The accepted generated code records cycle size and charges the recursion budget accordingly. `input-resource-limit-conventions.md` now records that generated recursion accounting must correspond to actual stack/call expansion, and that generator-owned patterns should be fixed in the generator rather than hand-patched across generated dissectors.

- **!13817 — proxy code must not assume one concrete source-model subclass when several legitimate backends exist.** John Thacker's merged Windows crash fix distinguishes `InterfaceTreeModel` from `InterfaceTreeCacheModel` with safe Qt type checks rather than calling a method through an invalid concrete cast. `qt-model-conventions.md` now records the multiple-source-backend rule and recommends testing the proxy with every supported source model and initialization state.

## Strong corroborating or negative findings retained without duplicating notebook rules

- **!13862** is the master version of the GLib-version guard already seen in the prior batch's backport: compatibility workarounds should be gated by the dependency version whose behavior requires them rather than applied indefinitely.
- **!13859/!13861** reinforce full initialization of structs before hashing, comparison, or later state use; initializing only fields currently read is fragile as structures evolve.
- **!13858** includes direct clang static-analyzer cleanup during review, reinforcing that analyzer findings should be investigated and fixed when they expose real dead/uninitialized logic rather than mechanically silenced.
- **!13857** reinforces typed-field/value-mask checker coverage: accidental zero masks can silently change field display/filter semantics and should be caught by automated source checks.
- **!13837** reinforces using `pinfo->pool` for packet-lifetime dissector allocations and removing allocations whose returned text is not actually needed.
- **!13835** shows that fuzz/Valgrind wrapper option composition is itself part of test-harness correctness: one mode's implicit flags must not leave unsupported/conflicting options active in another mode.
- **!13825 versus closed !13815** is useful review evidence for preferring a protocol-correct generalized dispatch rule over a special case tied only to one observed descriptor triple. The accepted change tries USB protocol dissection at both the device and interface levels, and the original contributor verified it against the concrete capture that motivated !13815.
- **!13824** reinforces `check_typed_item_calls.py --check-value-string-checks` as a practical way to find missing value-string entries and documents intentional unassigned values instead of leaving gaps unexplained.
- **!13816 with stable !13823** records careful handling of an ambiguous IEEE 802.1Q definition. The discussion explicitly compares table layout text with a contradictory validation note rather than treating one sentence as sufficient; this corroborates the existing standards/source-of-truth convention to document ambiguity and interpret the full normative context.
- **!13822** adds a generator that consumes Bluetooth SIG YAML for UUID/company value strings, reinforcing the preference for reproducible generation from authoritative upstream data instead of repeated hand transcription.
- **!13814 and !13813** are stable backports of the IEC 61850 sampled-values validity update. They reinforce keeping generated/template and checked-in dissector output synchronized when a standards-derived semantic value changes.

The remaining MRs were release copyright updates, automated data/release-note refreshes, stable backports, protocol-specific correctness fixes, and small cleanups that did not establish a stronger durable rule than the items above. Merged changes were weighted more heavily than the single superseded closed proposal.

## Notebook commits from this run

- `653756d2b5553fd5ca5bc89f15b4a2a6ed15d9d8` — add subprocess IPC/lifecycle conventions from !13860, !13844, and !13846.
- `f337c6bab13b52a6f92c2cb86f9cc5b85b5d7caf` — add recoverable generic-API assertion/output-contract rules from !13853.
- `aa7d8558f692fe409ac0b5043c78e10e2f9b75fb` — add environmental/remote diagnostic ownership classification from Guy Harris's !13828 series.
- `844e9c7cbac36f44c065b4ac525d6d7bd0b7d043` — refine recursion-depth accounting from !13845 and Evan Huus's review.
- `a87a7c8ec078c9251d728f6d74fef83411ed3f6b` — add Qt proxy/source concrete-type safety rule from !13817.

## Frontier

`mr_13812.json` exists in corpus commit `ddcaa22b51c68f594e425a23388c3a2086813054` and is merged. It was fetched only to verify that the corpus continues and was **not** counted as reviewed. Therefore the corpus is not exhausted; absent newly scraped higher-numbered unreviewed material, !13812 is the next descending candidate.
