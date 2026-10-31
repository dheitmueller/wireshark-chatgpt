# Wireshark Documentation Conventions

## Describe actual platform and packaging-specific paths

Merged master !9613 is authored by Guy Harris and revises Wireshark, TShark, Dumpcap, and Rawshark man pages together. It avoids vague claims about “UNIX” versus Windows; specifies which UNIX-compatible platforms the guidance covers; distinguishes macOS application-bundle resources from traditional system or locally installed `/usr/share` / `/usr/local/share` layouts; and accounts for current per-user preferences and their platform-specific locations. The same maintenance also removes obsolete explanations for interface-listing commands that modern users do not need.

**Rule:** when documenting paths, preferences, platform-specific availability, or command-line behavior, identify the installation and OS assumptions explicitly. Keep related CLI man pages consistent and describe the executable's actual search/override rules rather than inventing one universal filesystem layout. Remove examples that are misleading, obsolete, or require unsupported environments. **Confidence:** very high, merged Guy Harris-authored documentation update.

## Documentation paths and build-system case must stay synchronized

Merged master !9633 renames guide source files consistently; its preparatory work was backported in !9645/!9646, and the matching `CMakeLists.txt` case correction was merged in !9648 with backports !9649/!9650. **Rule:** when renaming documentation sources, update cross-references, source includes, generated paths and CMake source lists together; verify the change on case-sensitive and case-insensitive filesystems and across maintained branches. Backports are corroboration of one master change, not separate independent conventions. **Confidence:** high.
