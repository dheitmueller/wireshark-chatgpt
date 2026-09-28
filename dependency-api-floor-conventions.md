# Wireshark Dependency API-Floor Conventions

## New API use must fit the declared minimum dependency version

During merged MR !6385, John Thacker identifies g_string_replace as requiring GLib 2.68, newer than Wireshark's declared minimum at that time and newer than GLib provided by supported platforms including RHEL 8 and Debian Bullseye.

**Rule:** before adopting a dependency helper, verify the version in which it was introduced against Wireshark's declared minimum. Local compilation against a newer SDK is not sufficient evidence of portability.

**Implementation options:** use an older supported API, add a project compatibility helper when justified, or deliberately raise the project minimum through the normal dependency-floor process. Do not accidentally raise the floor in an unrelated feature.

## Verify cross-platform assumptions about dependency output

In the same MR, Guy Harris corrects the claim that the word "version" in pcap_get_lib_version() is Windows-specific; UNIX-like platforms and Haiku also produce it.

**Rule:** before moving normalization or parsing into platform-specific code, verify the external API's output contract on the supported platform family. Keep generic handling generic when the behavior is not actually platform-specific.

**Confidence:** Very high. Direct John Thacker and Guy Harris review on merged work.


## Keep the documented dependency floor and the APIs actually used in sync

Merged MR !6140 corrects Wireshark's documented GLib minimum to 2.38. Merged MR !6138, authored by John Thacker, then removes use of `g_utf8_validate_len()`, which requires GLib 2.60, and returns to `g_utf8_validate()` so supported CentOS 7 builds continue to work.

**Rule:** treat the declared minimum dependency version as an API ceiling for ordinary feature work. Documentation, CI/platform support, and helper selection must agree; an API that exists on the developer's machine is not sufficient evidence that it is available on every supported build platform.

**Confidence:** Very high. Two merged master changes, including a direct compatibility correction by John Thacker.
