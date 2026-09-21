# Reviewed MRs automation run: !14063–!14112

## Corpus snapshot

- MR corpus: `dheitmueller/wireshark-corpus-mrs`
- Corpus commit reviewed: `ddcaa22b51c68f594e425a23388c3a2086813054`
- Notebook starting commit: `1aa2a48e7fa317dc3ad06e2db6ae98628c9703e5`

## Selection method

The already-reviewed MR set was reconstructed from all available review tracking in `dheitmueller/wireshark-chatgpt`, including `reviewed-mrs.md` and the per-run files in `reviewed-mrs-automation/`. The historical !17571–!17620 batch remains explicitly part of the reviewed set. The corpus and notebook had not advanced since the preceding exact !14113–!14162 run; that run explicitly fetched !14112 only to verify the frontier and did not count it as reviewed. Exact MR-number set subtraction therefore selected the fifty highest-numbered corpus MRs not already in the reviewed set. No range was treated as reviewed merely because neighboring MRs appeared in a ledger.

## Exact MRs reviewed

!14112, !14111, !14110, !14109, !14108, !14107, !14106, !14105, !14104, !14103, !14102, !14101, !14100, !14099, !14098, !14097, !14096, !14095, !14094, !14093, !14092, !14091, !14090, !14089, !14088, !14087, !14086, !14085, !14084, !14083, !14082, !14081, !14080, !14079, !14078, !14077, !14076, !14075, !14074, !14073, !14072, !14071, !14070, !14069, !14068, !14067, !14066, !14065, !14064, !14063.

Exactly 50 MRs were reviewed in this run. All 50 were merged; there were no open or closed-unmerged MRs in the selected batch.

## Durable findings promoted to the notebook

### Stable protocol-layer accounting across redissection

Merged master !14101 and !14106, both authored by John Thacker, expose a subtle state/reassembly invariant. A dissector can be invoked during fragmentation/desegmentation, return nonzero, and still consume no bytes or add no retained protocol-tree contribution. If that transient invocation permanently consumes a slot in `pinfo->layers`, later PDUs in the same frame can receive different layer numbers on the first pass and on redissection. !14101 demonstrates the resulting HTTP/2/TLS reassembly failure; !14106 generalizes the fix by rolling back the layer entry for a non-consuming invocation. Peter Wu's review also stresses that the rollback must remove the correct failed/current layer rather than an unrelated fallback layer.

A new `protocol-layer-accounting-conventions.md` records the rule that layer identity must be stable across passes and should represent retained semantic protocol contribution rather than mere invocation. It also distinguishes this concern from the existing explicit semantic-nesting-depth guidance.

### Generated and hand-maintained source boundaries can coexist inside one dissector

Merged master !14100 contains high-authority Guy Harris guidance on the X11 dissector's source-of-truth boundary. X11 registration data is generated, while the packet dissection logic involved in the bug is hand-written. The immediate fix correctly changes the hand-maintained code. Guy also describes the preferable longer-term architecture: generate mechanical protocol structure from the XML specification and express intentional Wireshark-specific behavior through an explicit `.cnf`-style override mechanism.

`authoritative-source-preservation-conventions.md` now records that generation ownership must be determined at the artifact/behavior level rather than assuming an entire subsystem is generated or manual.

### Parent processes should interpret child exit statuses semantically

Merged master !14085 fixes a noisy capture-statistics warning for users with no capture interfaces. John Thacker's review steers the implementation toward the specific project-standard `WS_EXIT_NO_INTERFACES` status: that expected environment condition can be quiet while unrelated nonzero exits remain visible as real failures. The durable rule is not to suppress all subprocess failures merely because one expected outcome is noisy.

`cli-exit-status-conventions.md` now records that standardized child exit statuses are part of the parent/child protocol and should drive severity/recovery policy by failure class.

### Verify whether a tree/display change is actually cosmetic

Merged !14102 reorders IEEE 802.15.4 source/destination address presentation. Graham Bloice explicitly raises the risk of users post-processing capture output and distinguishes a genuinely display-only change from one that changes fields or TShark-visible output. The accepted visible change is release-noted. Merged !14097 independently changes truncated-field rendering and likewise documents the user-visible behavior.

`protocol-tree-hierarchy-conventions.md` now records that tree presentation changes should be checked against GUI, field, TShark/export, and display-filter surfaces before being classified as cosmetic.

## Strong corroborating findings not duplicated in notebook rules

- !14112 adds Wi-Fi decode-as support for UDP and includes a concrete reference capture, reinforcing existing decode-as/integration-test practice.
- !14111, !14110, and !14109 are supported-branch backports of the X11 ConfigureWindow correction represented by !14100; they reinforce the merged master source-of-truth lesson without adding a separate rule.
- !14108 improves extcap/libssh passphrase and authentication diagnostics, reinforcing the existing practice of distinguishing actionable failure classes rather than presenting a generic connection failure.
- !14107 removes legacy PCRE1 setup once supported platform/dependency baselines provide PCRE2, reinforcing dependency-baseline cleanup guidance.
- !14105 maps Wireshark logging levels to libssh logging priorities rather than treating external-library log levels as unrelated defaults.
- !14104 fixes Qt/extcap ownership leaks, reinforcing existing explicit ownership/lifetime guidance.
- !14099 preserves obsolete/recent preference migration behavior and reinforces existing persisted-configuration compatibility guidance.
- !14098 raises the minimum libssh requirement based on supported platform baselines and removes obsolete compatibility code.
- !14096, !14095, and !14094 are stable backports of !14087's NetFlow zero-length variable-field fix. !14087 treats zero length as a valid value and advances safely, reinforcing existing parser-progress guidance without misclassifying every zero length as malformed.
- !14093, !14092, and !14091 are stable backports of ISUP recursion guards and reinforce the existing bounded-recursion rules.
- !14089 extends `check_val_to_str.py` coverage to generated dissectors and updates generated/source inputs together, reinforcing checker/source-generation guidance.
- !14078 fixes compatibility with older libssh. John Thacker corrects the version guard so it corresponds to the dependency version where SHA-1 behavior actually changed rather than an arbitrary stricter cutoff; this corroborates existing dependency/version-boundary guidance.
- !14076's SMB2 review catches duplicate/incompatible field registrations and an item-length/type mismatch, reinforcing typed-item/field-width checks.
- !14070 and !14068 extend heuristic dissector tables with human-readable descriptions, reinforcing the existing distinction between machine names and user-facing descriptions. The post-merge !14070 discussion also explicitly considers third-party plugin source compatibility before renaming a C API.
- !14066 extends `tshark -G dissector-tables` with heuristic table information and updates the man page to document the machine-readable fields, reinforcing documented output-contract practice.
- !14074, !14067, !14065, !14064, and !14063 are authoritative 3GPP ASN.1 dissector updates and reinforce the existing generated-source synchronization conventions.

## Frontier

`mr_14062.json` exists at the corpus commit above and is merged. It was fetched only to verify that the corpus continues and was **not** counted or ledgered as reviewed. Unless a later corpus snapshot introduces a higher-numbered unreviewed MR, !14062 is the next descending candidate.
