# Wireshark CI Resource-Scheduling Conventions

This file records durable conventions for CI resource serialization and recurring long-running validation. Current upstream CI configuration remains authoritative.

## Do not make independent scheduled fuzzing streams serialize accidentally

CI resource groups are a correctness and capacity control, not merely a label. Jobs that share a resource group are deliberately serialized, so independent recurring validation streams should not share one unless mutual exclusion is actually required. In particular, maintained branches that each need regular fuzzing should have branch/workload-specific serialization keys; otherwise one branch can consume the slot while another repeatedly falls behind.

Merged release-4.6 MR !21254, authored by John Thacker and approved and merged by Gerald Combs, gives the release branch its own fuzz and Valgrind-fuzz resource groups instead of reusing the master-branch groups. The stated failure mode was that the two daily fuzz schedules would take turns under a shared group, causing the intended fuzzing cadence on both branches to fall behind.

**Implementation rule:** define the scope of every CI serialization key from the resource that truly must be exclusive. If master and supported release branches can safely fuzz independently, give each branch/workload its own resource group. Share a resource group only when concurrent execution would conflict on an actually shared scarce resource or state.

**Review rule:** when adding a scheduled or long-running job to another maintained branch, audit copied `resource_group` values rather than inheriting them mechanically. Verify that the resulting scheduling topology still lets every branch run at its intended cadence.

**Confidence:** Very high. Merged CI correction authored by John Thacker and accepted by Gerald Combs, with the starvation/backlog behavior explicitly documented.
## Match expensive packaging validation cadence to its per-change value

Not every useful CI job needs to run in parallel with every merge. When a packaging workload is expensive, poorly cacheable, and provides little additional per-commit signal, keep the validation but move it to a deliberate recurring schedule rather than consuming merge-pipeline capacity continuously.

Merged master MR !8618, authored and merged by Gerald Combs, moves the Linux RPM and APT package builds to a twice-daily schedule. The MR records the concrete resource reasons: RPM uses versioned build directories that undermine ccache effectiveness, while the APT package build is simply slow; running both with every merge was wasting CI minutes. Merged !8619 follows by moving the related RPM test jobs to the same twice-daily cadence.

**CI rule:** decide cadence from cost, cache behavior, and how quickly a regression must be detected. Expensive distribution/package integration checks can remain valuable scheduled coverage even when they are not efficient merge gates.

**Review rule:** when removing a job from per-merge execution, verify that it is still exercised on an explicit schedule and that faster compiler/unit/static checks continue to provide immediate merge-request feedback. Treat reducing cadence as a coverage-topology change, not as silently deleting validation.

**Confidence:** Very high. Merged CI policy changes authored and merged by Gerald Combs with the cost/caching rationale stated directly.
