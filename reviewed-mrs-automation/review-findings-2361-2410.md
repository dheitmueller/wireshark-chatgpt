# Wireshark MR review findings — !2361 through !2410

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are weighted more heavily than closed/unmerged work. Guy Harris-authored changes and direct maintainer review are treated as especially authoritative.

## Strong durable findings

### Wiretap encapsulation domains are distinct contracts (!2369, !2390, !2391)

John Thacker's merged !2369 and Guy Harris's merged stable counterparts !2390/!2391 fix Export-PDU code that fed a pcap LINKTYPE value into `wtap_rec.packet_header.pkt_encap`, which expects a `WTAP_ENCAP_*` value. The accepted code keeps in-memory record/interface state in the Wiretap encapsulation domain and lets format-specific writers translate when necessary.

### pcapng records must agree with their interface description (!2388, !2392, !2393)

Guy Harris's merged master fix and accepted backports reject a packet whose `pkt_encap` differs from the selected interface's encapsulation. The detailed versions return `WTAP_ERR_INTERNAL` text identifying the interface and both encapsulations.

### Export semantic capability queries, keep format-number translation private (!2404)

Guy Harris made `wtap_wtap_encap_to_pcap_encap()` private to libwiretap and exported `wtap_dump_can_write_encap()` instead. Callers should ask the semantic question they need rather than learn an implementation-specific LINKTYPE mapping.

### Keep compatibility scaffolding out of public ABI (!2387)

Guy Harris removed `wtap_register_backwards_compatibility_lua_name` from the public symbol list because it is only for built-in file modules supporting an old Lua compatibility mechanism, not plugins.

### Error detail has ownership semantics (!2389, !2394)

Guy Harris added `WTAP_ERR_INTERNAL` to paths that free `err_info`, because that error can also return allocated explanatory text.

### Standard field APIs beat bespoke formatting layers (!2405)

The large merged NVMe Identify Controller MR went through extensive review by Alexis La Goutte, Anders Broman, and Pascal Quantin. Review pushed it toward `VALS()`, `BASE_CUSTOM`/unit metadata where appropriate, standard bitmask APIs, and `FT_BOOLEAN` for one-bit fields. Pascal specifically corrected the registration model: a masked `FT_BOOLEAN` uses the containing field width (8/16/32) plus the mask, not `BASE_HEX`.

### Fetch/register once and reuse (!2363)

Anders Broman recommended `proto_tree_add_item_ret_uint()` when the IEEE 802.11 FTM trigger also had to be used for column text instead of separately fetching the byte and adding it to the tree.

### Informational CLI queries are not errors (!2381–!2383)

Guy Harris's TShark series accepts `-U ?` to list valid Export-PDU taps, factors listing into a helper, and deliberately stops reporting the list through the generic command-line error path.

### Preserve native OS errors when errno is lossy (!2374–!2376)

Guy Harris documents that a closed Windows pipe may become either `EPIPE` or `EINVAL` depending on the underlying Windows error. The accepted implementation consults `_doserrno`, recognizes `ERROR_NO_DATA` as the same normal pipeline-close condition, and uses `win32strerror(_doserrno)` for other failures.

### Local hooks should reuse canonical validation (!2378)

The merged commit-msg hook is a thin adapter to `tools/validate-commit.py`; Pascal Quantin's review exposed Windows invocation/shebang portability. Local validation should reuse the repository's authoritative checker and be tested on supported developer platforms.

## Additional reviewed evidence

- !2410, !2409, !2407: Guy Harris keeps equivalent Windows/non-Windows dialog captions semantically aligned and fixes singular/plural wording.
- !2408: IEEE 802.11 tag-table and tag-length fixes found by WFA testing.
- !2406: IEEE 1905 boolean mask corrected from 0x20 to 0x80 after external testing.
- !2403: Guy Harris updates stale Export-PDU comments to describe the actual interface-information requirement.
- !2402/!2401: Guy Harris removes caller-owned Export-PDU state that is purely local implementation detail.
- !2400: John Thacker fixes a backport that accidentally retained a line not present in the correct release-3.4 adaptation.
- !2399: closed/unmerged Protobuf/PFCP proposal; down-weighted.
- !2398/!2396/!2395: automatic release/data/translation updates; scanned, no durable lesson.
- !2397: voice-dialog action role/order/naming cleanup; Windows tested; macOS CI unavailable at the time.
- !2386: RK512 dissector ultimately merged after sample-capture, release-note, and pre-commit cleanup.
- !2385: dumpcap threaded path avoids double-incrementing the received counter.
- !2384: Protected FTM support follows a preparatory refactor of shared request/response parsing.
- !2380: TShark `-G` option-order fix; review explicitly considers compatibility cost before changing long-lived CLI behavior.
- !2379: voice-dialog selection commands use consistent actions and shortcuts.
- !2377: ICMPv6 RFC 5837 extension parsing; merged.
- !2373: Guy Harris again establishes `G_GUINT64_FORMAT` rather than assuming `%lu` for `guint64`.
- !2372: spelling cleanup accepted after confirmation with the original contributor.
- !2371: Guy Harris regularizes capture-format documentation and distinguishes native from imported formats.
- !2370/!2367: RTP/VP8 spec-description cleanups.
- !2368: Gerald Combs makes Debian CI run the versioning script so package metadata matches the build.
- !2366: closed/unmerged attempt to synthesize an absolute captured-frame number from drop counts; down-weighted.
- !2365: EAP leak fix frees the token vector.
- !2364: NAN bit-offset arithmetic widened from 8 to 32 bits.
- !2362: Guy Harris renames the Observer Wiretap module after the file format/product rather than corporate lineage.
- !2361: GitLab CI shallow-fetch tuning balances clone speed with the need for a reachable tag for `git describe`.

No SMPTE ST 291/VANC packet type was encountered.
