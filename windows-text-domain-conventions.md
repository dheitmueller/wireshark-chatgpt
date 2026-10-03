# Windows Text-Domain Conventions

## Do not infer all Windows encodings from the C runtime locale

Merged master MR !91, authored and merged by Guy Harris, removes a redundant and harmful conversion of `_tzname[]`: Wireshark had already selected UTF-8 for the Visual Studio C runtime with `setlocale(LC_ALL, ".UTF-8")`, so those CRT strings were already UTF-8. Guy explicitly notes that this does not make Windows "ANSI" APIs or non-UTF-16 console output UTF-8. MR !91 itself left a Windows build error by referring to a removed cache; merged !103 immediately corrected the final code to return `_tzname[tmp->tm_isdst]` directly.

**Implementation rule:** identify the text domain that produced a string—Visual Studio CRT locale, UTF-16 Win32 API, ANSI/system code page, or console—and convert at that boundary. Configuration of one Windows text domain does not silently change the others.

**Evidence weight:** Extremely high. Direct merged-master rationale from Guy Harris, with !103 establishing the final buildable implementation.
