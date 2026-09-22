# Wireshark Packaging Conventions

This file records durable packaging and downstream-integration conventions extracted from upstream Wireshark merge-request review. Current distribution policy and upstream build/install definitions remain authoritative.

## Moving installed files between binary packages requires an explicit upgrade relationship

A source-tree install-layout change can be correct for a fresh install while still breaking upgrades of split distribution packages. When a file moves from one Debian binary package to another, the old installed copy and the new package's copy overlap unless package metadata explicitly defines how the transition is resolved.

Merged master MR !13705 moved codec/plugin files between Wireshark Debian packages. Debian maintainer Balint Reczey pointed out after merge that safe upgrades require appropriate versioned `Breaks` + `Replaces`; otherwise `apt`/`dpkg` can stop on file-overlap conflicts and leave the user to repair the installation manually. Gerald Combs gave that downstream feedback strong project-level weight, explicitly noting that distribution-maintainer contributions benefit the larger Wireshark community and mission.

**Packaging rule:** whenever an MR changes which binary package owns an already-shipped path, review the upgrade path from the previous package version, not only the files produced by a clean build/install. Add the distribution-specific transition metadata required to transfer ownership safely.

**Review rule:** treat knowledgeable downstream packager feedback as compatibility evidence, not as an optional concern outside the source tree. A change to CMake install destinations, plugin locations, or package manifests can create real user-visible upgrade failures even when `make install` is internally consistent.

**Testing rule:** for package-ownership moves, test an actual upgrade from a version that owns the old path as well as a clean install of the new packages.

**Confidence:** Very high for Debian package transitions: the requirement was stated directly by a Debian maintainer and reinforced at project level by Gerald Combs after a merged change exposed the issue.

## Test relocatable pkg-config paths against multiarch layouts and external consumers

A relative `pkg-config` path is not inherently relocatable if it assumes a fixed number of parent directories between the `.pc` file and the installation prefix. Distribution multiarch layouts can insert additional path components, so a formula that works under a default libdir can point at the wrong prefix downstream.

Merged master MR !13671 changed Wireshark's `.pc` files to derive paths from `pcfiledir` so installs staged with a differing `DESTDIR` would remain usable. Later downstream testing reported that Debian installs the file in a multiarch location and therefore required another `..` component; the original relative-depth assumption broke an externally maintained libvirt Wireshark plugin. That later feedback is important evidence about the accepted change's portability boundary even though it arrived after merge.

**Implementation rule:** derive relocatable metadata from install variables whose relationship is defined by the build/package system; do not assume a fixed directory depth unless that depth is itself an enforced install contract.

**Testing rule:** validate generated `.pc` files in default, custom-prefix, staged `DESTDIR`, and representative multiarch layouts. Include at least one external consumer build so a syntactically valid `.pc` file is also shown to advertise correct include/library/plugin paths.

**Confidence:** High. The merged change had a sound relocatability goal, but later concrete Debian multiarch/external-plugin breakage demonstrates that fixed relative-depth assumptions require broader layout testing.