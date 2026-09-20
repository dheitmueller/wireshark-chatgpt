# Wireshark CI Dependency Conventions

This file records durable conventions for where CI-only tooling dependencies should live. Current upstream CI configuration and development-container definitions remain authoritative.

## Keep narrowly used CI dependencies local to the job

Do not automatically add every CI tool to Wireshark's shared development image. A dependency needed by one narrow checker or pipeline job can be installed by that job, keeping the common image focused on broadly shared build and development requirements.

Merged MR !15860 adds the spelling checker to CI. Martin Mathieson asked whether `pyspellchecker` should be added to the Ubuntu development image. Gerald Combs recommended installing it immediately before the spelling job and stated that, although the shared image could carry it, he tends to avoid doing so when a package is a dependency for a single job. The accepted pipeline followed the job-local approach.

**CI rule:** place a dependency in a shared development/CI image when it is broadly reused, part of the normal development/build environment, or there is another concrete reason for centralizing it. For a tool used by one focused job, prefer job-local installation unless reproducibility, availability, or execution-cost requirements make that impractical.

**Review rule:** when adding a CI checker, review both the checker semantics and the dependency-placement cost. Shared images are an architectural dependency surface, not merely a convenient package cache.

**Confidence:** Very high. Direct CI-maintainer guidance from Gerald Combs on a merged master MR, followed by the accepted implementation.
