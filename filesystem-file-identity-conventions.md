# Wireshark Filesystem File-Identity Conventions

This file records durable conventions for determining whether two paths refer to the same filesystem object. Current upstream source remains authoritative.

## Compare file identity, not path spelling, when aliasing affects correctness

Two different path strings can name the same underlying file through hard links, relative/absolute aliases, or other filesystem naming mechanisms. When treating two files as distinct could destroy or overwrite data, pathname equality or normalized-path equality is not a sufficient safety check.

Merged master MR !14728, authored and merged by John Thacker, changes the Windows `files_identical()` implementation from `_fullpath()` string comparison to filesystem identity using `GetFileInformationByHandleEx(FileIdInfo)`, comparing the volume serial number and file ID. Besides handling NTFS hard links, the implementation no longer treats two empty path strings as though they identify the same file. The Unix implementation already uses filesystem metadata identity rather than path text.

Merged master MR !14730, also authored by John Thacker and approved/merged by Anders Broman, uses `files_identical()` to ensure the TLS debug file and TLS key-log file do not resolve to the same file. If they do, Wireshark reports the error and nulls the debug filename before the later setup path can open and overwrite the key-log file.

**Implementation rule:** when correctness or data safety depends on two filenames referring to different files, compare the underlying file identity (for example device/inode on Unix or volume plus file ID on Windows), not merely normalized pathname strings.

**Failure rule:** perform destructive alias checks before opening or truncating output and move the operation into a safe state immediately on collision. Reporting the error after a destructive open is too late.

**Confidence:** Very high. Two merged master changes by John Thacker, including a concrete data-loss prevention use of the identity helper and senior-maintainer acceptance.