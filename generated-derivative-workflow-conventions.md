# Wireshark Generated-Derivative Workflow Conventions

This file records durable conventions for files generated from authoritative metadata but maintained by a separate project automation workflow. Current upstream automation remains authoritative.

## Edit the authoritative input, not a derivative that automation owns

Merged master MR !9418 originally attempted to correct an entry in `AUTHORS`. Pascal Quantin pointed out that `AUTHORS` is generated and directed the contributor to change `.mailmap` instead. The accepted MR contains only the authoritative `.mailmap` correction and was squashed before merge.

Merged master MR !9451 changes `tools/make-sminmpec.py` so enterprise names using "previously" are handled like "formerly". Gilbert Ramirez initially asked for the conflicted generated TSV to be updated, but Jaap Keuter gave the more specific project-workflow guidance: drop `enterprise.tsv` from the MR because the normal automatic update will regenerate it. The contributor did so and the MR merged.

**Implementation rule:** first determine who owns regeneration for a derivative. If the project's established scheduled or bot workflow owns it, submit the authoritative metadata or generator change and let that workflow update the derivative rather than hand-editing or unnecessarily carrying the generated copy.

**Review rule:** distinguish this case from generated source that CI expects to be regenerated and committed in the same MR. The repository's ownership and update contract for that artifact decides the workflow.

**Submission rule:** when review redirects a change to the actual source-of-truth file, clean up and squash the MR so the final history presents the accepted authoritative change.

**Confidence:** Very high. Two merged master examples with explicit maintainer guidance from Pascal Quantin and Jaap Keuter.
