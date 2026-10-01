# Wireshark MR review findings — !2311 through !2360

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are weighted more heavily than closed/unmerged work. Guy Harris-authored changes and direct maintainer review are given especially high architectural weight.

## Strong durable findings

### Keep format probing separate from normal record reading (!2349, !2350; precursor !2335/!2342)

Guy Harris's merged master !2349 moves first-Section-Header-Block recognition into `pcapng_open()` instead of forcing the ordinary block reader to serve both as a format detector and as the parser for an already accepted pcapng file. Once the format is accepted, `pcapng_read_block()` can return a simple success/failure result and treat malformed/short blocks as read or bad-file errors. The special interpretation "this is not my file format" belongs to open/probe logic. !2350 is the release-3.4 backport. Earlier !2335/!2342 are useful transitional steps but the later design is stronger evidence.

### Let the format module own its compatibility aliases (!2359)

Guy Harris removes a central table of old file-type/subtype names and adds registration of compatibility aliases beside the libpcap format's own registration. A compatibility name is format-specific policy; keeping it with the format avoids a second central registry that must be edited in lockstep with the owner module.

### Capture metadata is evidence, not automatically ground truth (!2311, !2323, !2332)

Guy Harris's merged 802.11 series shows that capture headers can conflate channel properties with per-packet modulation. PPI/radiotap-derived channel flags are not trusted universally when captures contradict them. The accepted code uses more reliable per-packet evidence such as extension headers and data rate, then uses frequency/channel only where it can disambiguate 11a vs 11g. If a property such as short-preamble state is unavailable, the pseudo-header marks it unavailable instead of fabricating it. The same principle is applied across multiple file/header formats in !2323 and specifically to CommView in !2332.

### Named dissector registration is an explicit direct-lookup extension point (!2322)

Guy Harris explains that a dissector is not "incorrectly registered" merely because it lacks a named `register_dissector()` entry. A dissector invoked through an integer/string dissector table is already callable through that dispatch mechanism. Named registration is appropriate when another component needs the specific dissector directly, such as Lua `Dissector.get("dect")`. The accepted change adds the name because the contributor had that concrete direct-lookup use case.

### Fix real interface-contract bugs even when a new compiler first exposes them (!2324)

A GCC 11 build exposed mismatches between function declarations and definitions. Guy Harris explicitly states that true declaration/definition mismatches should be fixed even when detected by an alpha/beta compiler. Peter Wu adds the complementary review rule: do not mechanically remove array extents merely to silence the compiler; verify that the documented bounds reflect actual call contracts first.

### Prefer familiar Wireshark dissection APIs over a private mini-DSL when readability suffers (!2339)

The large NVMe Identify Controller expansion initially introduced a table-driven private abstraction for ordinary field additions. Anders Broman said the result was harder to read than direct `proto_tree_add_item()` calls and preferred conventional offset advancement; Alexis La Goutte agreed and pointed to existing helpers such as `ptvcursor` and bitmask APIs. The contributor reworked the change. The lesson is not "never abstract": use standard Wireshark abstractions first and reserve local helpers for genuine repeated structure rather than replacing straightforward dissection with an unfamiliar local language.

### Defaults for heuristic/configured dispatch must be conservative (!2356)

AUTOSAR NM's default CAN-ID mask was zero, making every comparison true and causing all CAN frames to enter the dissector when no configuration existed. The merged fix uses an all-ones mask so the unconfigured state does not claim the entire input domain. A disabled/unconfigured dissector default should not accidentally become a catch-all heuristic.

### Model a presence flag and the value it guards as distinct fields (!2360)

RFC 5837 encodes an MTU-presence bit and, when present, a 32-bit MTU value. The accepted ICMP change separates `icmp.int_info.mtu_present` from `icmp.int_info.mtu`, rather than overloading one registered field for both meanings. It also corrects offset and IPv6 subtree-length handling in the same variable-layout object.

### Reduce platform build failures to minimal include-resolution reproducers (!2328)

Peter Wu reduced a macOS Qt5/Qt6 Homebrew conflict to a tiny standalone CMake project and compiler command. This demonstrated that an unrelated `-isystem /usr/local/include` could outrank the Qt5 framework header path and pull Qt6 headers into a Qt5 build. The accepted workaround then targets the demonstrated include-order mechanism instead of guessing from the original large build.

### Optional typed data must be interpreted only after its type is known (!2345, !2346)

Guy Harris's pcapng `if_filter` fixes use the leading type byte to determine how the remainder is interpreted. Recognized type 0 is a printable libpcap filter string; recognized type 1 is BPF instruction data; unknown types are ignored rather than being coerced into a known representation. The dissection cleanup also prefers `proto_tree_add_item_ret_*` and display-string APIs when the value is immediately needed or must be rendered safely.

## Additional reviewed evidence

- !2358: Guy Harris removes redundant pcapng-open initialization after documenting which values the SHB reader itself establishes.
- !2357: Guy updates Observer corporate/product naming after ownership changes.
- !2355: EAP memory leak fix; review identifies release-3.4 as the needed stable backport.
- !2354: WSLua gains `DissectorTable.try_heuristics()`; Stig Bjørlykke requests a test and the contributor adds one. A later post-merge report warns that invoking a heuristic list from a dissector registered on that same list can recurse indefinitely; treat that as a reentrancy caution rather than an accepted architectural rule.
- !2353: closed/unmerged OSCORE libsodium proposal. Pascal Quantin argues for reusing an existing dependency when possible because every new crypto library adds Linux/macOS/Windows integration burden. Useful negative dependency evidence only.
- !2352: closed/superseded preallocation bump; no independent implementation weight.
- !2351: Clang Analyzer warning cleanup, merged.
- !2348: VP8 version-field details, merged after domain review.
- !2347: RTP Player shortcut additions, merged.
- !2344: LLDP permits valid organizational TLVs with no payload instead of throwing malformed-packet exceptions.
- !2343/!2338: Gerald Combs adds `tshark -G fields` artifacts to release CI branches to automate release work.
- !2341/!2340/!2337: DECT spelling corrections across branches.
- !2336: IEEE 802.11 PASN authentication identifier support, merged.
- !2334/!2333/!2331/!2330: version/release plumbing; merged, no durable new convention.
- !2329: NFS SP4_SSV correctly treats hash/encryption algorithms as OID arrays.
- !2327: NAN field-size correction fixes failed dissection.
- !2326: builds without Lua report an explicit error for `-X lua_script` instead of silently ignoring an impossible request.
- !2325: VP8 bit fix; Martin Mathieson argues that version-field values should explain their protocol meaning and cite the relevant RFC, not merely expose raw bits.
- !2321/!2320: Guy Harris cleans 802.11 Prism/radiotap terminology and clarifies which metadata represents center frequency vs modulation.
- !2319/!2318/!2317: Windows spandsp package update to use the Visual C++ intrinsic implementation.
- !2316: RTP Player can navigate to related signaling/SETUP packets.
- !2315: OAMPDU parses DPoE GetRequest messages for Link/User Port objects.
- !2314/!2312: TECMP timestamp bug fix on master and stable.
- !2313: RTP Player multi-selection support.

No SMPTE ST 291/VANC packet type was encountered.
