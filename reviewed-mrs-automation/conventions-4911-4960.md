# Conventions from Wireshark MRs 4911-4960

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

## Recursive protocol depth

Merged master !4954 limits Thrift nested-type parsing with per-protocol depth stored in packet_info, a configurable maximum, and expert reporting. The limit is also part of the subdissector option structure.

**Rule:** for a recursively nestable packet grammar, track semantic protocol depth around the child parse, enforce a sane limit before descending, and restore the depth on exit.

## Heuristic ownership checks

Merged master !4948, authored by John Thacker, strengthens RTP recognition with mandatory structural facts: captured bytes for probe reads, room for the fixed header/CSRC list and extension header, reserved RTP/RTCP payload-type separation, and padding validation only when the packet tail is actually captured. Explicit RTP selection remains available through SDP or Decode As.

**Rule:** a heuristic should cheaply establish enough protocol structure to avoid false positives without acting as the full conformance checker. Keep captured length and reported length distinct.

## Generated-data replacement

Merged master !4946 builds the PCI-ID source in memory, checks plausible minimum vendor/device counts, and writes the tracked generated artifact only after the checks pass. Stable !4952 corroborates it.

**Rule:** validate a complete fetched/generated dataset before replacing the prior known-good tracked artifact.

## Generated source of truth

Merged !4957 states that the ASTERIX C dissector is generated and should not be edited manually; its updater resolves templates relative to the script and supports a canonical update operation. Merged stable !4913 records the complementary failure mode: Skinny output had drifted after manual edits and the XML/template/generator had to be resynchronized.

**Rule:** change authoritative generator inputs and regeneration machinery, not only generated output, and keep automated regeneration deterministic and independent of the caller's working directory.

## Windows platform versus compiler checks

Merged master !4918 distinguishes platform predicates from compiler/toolchain predicates: _WIN32 and CMake WIN32 mean the Windows platform, while _MSC_VER/MSVC identify Microsoft's compiler and __MINGW32__/MINGW identify MinGW. Guy Harris's merged !4945 moves this guidance next to the other Windows portability material.

**Rule:** gate code on the property that matters; do not substitute an MSVC check for a Windows-wide condition or vice versa.

## Optional build targets

The master-origin fix is merged !4959 by Peter Wu: when Asciidoctor is unavailable, the manpages target is absent, so the Wireshark bundle depends on it only under the same capability condition. The previously reviewed !4963 is the release-3.6 backport; it was opened/merged by Guy Harris but carries Peter Wu's original commit.

**Rule:** if a feature condition controls creation of a build target, guard every dependency/reference to that target with the same condition.

## Bounded work without breaking protocol state

Merged master !4930 enables the RTPS implementation limit on packet-declared type elements. Review by the original contributor catches an earlier placement that would have stopped type-object processing too early and broken later nested DATA dissection.

**Rule:** bound packet-controlled work at the resource-consumption boundary while preserving the protocol state needed by later decoding.

## Filter-name style versus compatibility

Merged !4917 distinguishes filter names from C identifier abbreviations and removes a protocol-only uppercase restriction; lowercase remains preferred style. Later merged !4968 and !5010 provide stronger subsequent evidence for the same style/compatibility split.

**Rule:** prefer lowercase for new filter names, but do not turn style into a compatibility break for existing registered names without deliberate migration.
