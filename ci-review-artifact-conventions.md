# Wireshark CI Review-Artifact Conventions

This file records durable conventions for CI jobs intended to help reviewers validate source and documentation changes.

## Reuse the standard build environment and publish the thing reviewers need to inspect

A CI job should avoid rebuilding an ad hoc toolchain when the project's normal development image already contains what the job needs. It should also publish its meaningful output, not merely report that a command returned zero.

Merged master MR !111 adds a documentation build job. Peter Wu asks the contributor to reuse `wireshark/wireshark-ubuntu-dev` rather than building another environment, reducing job time substantially. The accepted job uses GitLab `rules:changes` so it runs for documentation/WSLua changes and publishes generated WSUG/WSDG HTML directories as artifacts, allowing reviewers to inspect rendered documentation.

**CI rule:** prefer maintained project images over bespoke per-job environment construction; scope expensive jobs to file classes that can affect their result; publish generated/rendered output when visual or structural review is part of correctness.

## Expand static checks over first-party code; exclude vendored code explicitly

Merged master MR !140 extends checkAPI coverage into `ui/qt`. The build configuration deliberately separates QCustomPlot headers/sources because that third-party code uses APIs prohibited by Wireshark's own checker.

**CI rule:** checker scope should follow code ownership. Bring first-party code under project policy checks; do not globally weaken a checker to accommodate vendored code. Isolate or explicitly exempt third-party sources when their upstream style/API choices are outside Wireshark's control.

**Confidence:** Very high. Both rules come from merged master CI/tooling changes; !111 has direct Peter Wu and Gerald Combs review.
