# Wireshark Windows Runtime Deployment Conventions

This file records durable Windows build/installer runtime conventions extracted from accepted upstream Wireshark changes. Current upstream build and installer files remain authoritative.

## Validate the deployed runtime against the newest toolchain used to build any component

The presence of a Windows C/C++ runtime is not sufficient by itself. When components are built with different supported Visual Studio toolsets, the installed Redistributable must be at least as new as the newest build tools used by any component. Installer logic should test that deployment contract explicitly instead of assuming the OS-provided runtime or an arbitrary older Redistributable is adequate.

Merged MR !26326, authored and merged by Gerald Combs, removes the deprecated Visual C++ Redistributable WiX merge modules and changes the installer to check for the required Redistributable version, failing clearly when the prerequisite is not met. The change also removes an Arm64 blocker because those merge modules are unavailable there.

During review, Guy Harris asked whether Wireshark could simply rely on the Universal CRT shipped with Windows. Gerald explained that this is not sufficient for the whole C++ runtime contract and cited Microsoft's compatibility requirement: the deployed Redistributable must be at least as new as the latest build tools used by any application component. Guy connected this to deployment-target reasoning on Darwin. Michael Mann also provided installer-maintenance context around migration to newer WiX versions.

**Implementation rule:** model the runtime as a versioned deployment prerequisite tied to the actual compiler/toolchain set. Prefer explicit installer prerequisite checks over deprecated merge-module bundling, and make failure visible at install time rather than allowing runtime incompatibility to surface after deployment.

**Confidence:** Extremely high. Merged master installer change authored/merged by Gerald Combs, with direct architectural discussion from Guy Harris and Michael Mann.