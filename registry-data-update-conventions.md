# Registry Data Update Conventions

Merged !1730 adds PCI ID lookup support. Gerald Combs and Alexis La Goutte steer the change toward a tools/make-pci-ids.py updater that fetches current upstream registry data, matching Wireshark's scheduled make-* update jobs. Jaap Keuter asks that implementation-only data structures stay in the C file.

**Rule:** externally maintained registry data should have a project-owned refresh tool consistent with existing make-* workflows. Keep internal indexing representation private and expose only the lookup interface consumers need.

**Confidence:** Very high. Merged change with direct maintainer review.
