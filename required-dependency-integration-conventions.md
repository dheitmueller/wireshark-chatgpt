# Required Dependency Integration Conventions

A dependency becoming required is an end-to-end repository integration change, not just a build-system declaration.

Merged master MR 5048 replaces the display-filter engine's GRegex/PCRE usage with PCRE2 and ultimately makes PCRE2 required. Review explicitly tracks Windows dependency bundles, setup scripts, project containers, macOS builders, and Windows installer packaging. A later automated Windows build exposed that the DLL still needed to be added to installer packaging, showing that a successful compile does not prove deployable runtime completeness.

Implementation rule: when promoting a dependency to required, audit every supported acquisition and delivery path: package discovery, bootstrap scripts, CI/container images, platform binary bundles, link targets, and final packaging/installers.

Validation rule: test at least one packaged/runtime artifact in addition to compilation.

Confidence: very high. Merged master migration with detailed maintainer review and follow-up packaging work.
