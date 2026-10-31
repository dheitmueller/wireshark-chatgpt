# Wireshark Documentation Conventions

## Describe actual platform and packaging-specific paths

Merged master !9613 is authored by Guy Harris and revises Wireshark, TShark, Dumpcap, and Rawshark man pages together. It avoids vague claims about “UNIX” versus Windows; specifies which UNIX-compatible platforms the guidance covers; distinguishes macOS application-bundle resources from traditional system or locally installed `/usr/share` / `/usr/local/share` layouts; and accounts for current per-user preferences and their platform-specific locations. The same maintenance also removes obsolete explanations for interface-listing commands that modern users do not need.

**Rule:** when documenting paths, preferences, platform-specific availability, or command-line behavior, identify the installation and OS assumptions explicitly. Keep related CLI man pages consistent and describe the executable's actual search/override rules rather than inventing one universal filesystem layout. Remove examples that are misleading, obsolete, or require unsupported environments. **Confidence:** very high, merged Guy Harris-authored documentation update.

## Documentation paths and build-system case must stay synchronized

Merged master !9633 renames guide source files consistently; its preparatory work was backported in !9645/!9646, and the matching `CMakeLists.txt` case correction was merged in !9648 with backports !9649/!9650. **Rule:** when renaming documentation sources, update cross-references, source includes, generated paths and CMake source lists together; verify the change on case-sensitive and case-insensitive filesystems and across maintained branches. Backports are corroboration of one master change, not separate independent conventions. **Confidence:** high.

## Align user-visible crypto algorithms with mandatory platform policy, and update documentation

**Merged master !9364** fixes capinfos aborting under FIPS 140-2 environments: libgcrypt rejects the RIPEMD-160 digest. **Guy Harris** raised the option of disabling FIPS mode; **Gerald Combs** suggested removing RIPEMD-160 altogether because its ecosystem use had declined. The author noted some environments require FIPS mode and cannot disable it. The accepted change removes the digest from the implementation, command-line help, and `doc/capinfos.adoc`; Gerald explicitly requested the documentation update.

**Compatibility rule:** avoid making a common inspection path fail because an optional legacy algorithm is forbidden by deployed security policy. Prefer a supported operation over assuming the user can override mandatory system policy. When the set of displayed algorithms/output columns changes, update command help, man pages, and any consumers/tests expecting the output schema. **Confidence:** very high; merged change with Guy Harris and Gerald Combs review.
