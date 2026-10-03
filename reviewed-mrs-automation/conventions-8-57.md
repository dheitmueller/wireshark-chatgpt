# Durable conventions from MRs 8 through 57

## State and redissection

When a UI action changes protocol preferences as well as a dissector-table selection, complete the preference update before redissection. Derived protocol state must not continue to reference replaced preference objects. MRs 26 and 53 provide the accepted Decode As example.

State must be scoped to the smallest unit that can vary independently. If a mode or protocol version can differ among devices or flows in the same capture, store it in conversation/association state rather than a process-global variable. MR 32 provides direct Pascal Quantin review for this rule.

## Protocol fidelity and generated code

Do not make malformed traffic look valid by silently substituting a different standards-defined field interpretation. Preserve the actual wire semantics and report violations explicitly when useful. Closed MR 54 is strong maintainer-review evidence from Anders Broman.

For generated dissectors, change the authoritative ASN.1/template/config input and regenerate the checked-in output. MRs 48 and 47 corroborate this.

## Field and source-byte fidelity

A registered field's type, byte width, encoding, highlighted extent, and parser-consumed value must describe the same wire object. If parser logic also needs a displayed integer, prefer the appropriate `proto_tree_add_item_ret_*` API so the tree and control value come from one decode. MRs 24, 51, and 52 support this.

## Reviewability and submission

Reviewability is an engineering constraint. Split a change when the review system cannot render it effectively. Attach representative captures to the issue or MR rather than committing capture or patch bundles into the source tree. Keep unrelated changes out of the MR and present a coherent logical history. MRs 32, 40, and 36 provide early direct review evidence.

## CI and automated analysis

A CI check should provide actionable signal for Wireshark's real languages and workflow. Prefer extending an existing job over duplicating it; publish artifacts that reviewers can use; and keep noisy or false-positive-prone analysis non-blocking unless project policy says otherwise. Tooling should enforce agreed policy rather than create it. MRs 22, 20, 23, and 18 support this.

## User-visible behavior

When a change adds new user-facing filter semantics or workflow-visible behavior, update release notes and user documentation where appropriate. MR 14 is the accepted example.
