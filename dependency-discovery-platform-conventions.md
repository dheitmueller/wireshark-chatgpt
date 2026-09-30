# Dependency Discovery and Platform Layout Conventions

Closed MR 3298 attempted to repair GLib discovery on RHEL by changing a global include hint. Gerald Combs showed that the change would break the Windows vcpkg layout and traced the Unix issue to search ordering around the library that CMake had selected. Merged master MR 3304 implements the accepted solution: on Unix, derive the glibconfig.h search root from the GLib library that was actually found; on Windows, preserve the vcpkg-specific layout contract.

Rule: when a dependency has architecture or configuration headers outside its public include directory, locate them relative to the selected library/package artifact or a documented platform package root. Do not replace one hard-coded cross-platform layout assumption with another.

Review rule: test discovery changes against more than the reporter's filesystem. A fix that solves one distro by changing a global hint may silently invalidate other package-manager layouts.

Confidence: very high. The rejected proposal has direct Gerald Combs review and the alternative merged as MR 3304.
