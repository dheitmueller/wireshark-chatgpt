# Review findings — Wireshark MRs !2161–!2210

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged work is weighted above closed/unmerged work. Maintainer-authored changes and substantive review comments receive additional weight, especially Guy Harris's architecture guidance.

| MR | Outcome | Review result |
|---:|---|---|
| !2210 | merged | Radius dictionary comment-only clarification; no new cross-cutting convention. |
| !2209 | merged | Vendor protocol updates; protocol-specific maintenance. |
| !2208 | merged | Adds SRT protocol-reference documentation; no coding convention. |
| !2207 | merged | macOS SDK-version handling cleanup; platform maintenance. |
| !2206 | merged | PTPv2 dissection updates. Anders Broman caught trailing whitespace; otherwise protocol-specific. |
| !2205 | merged | TFTP comment correction only. |
| !2204 | merged | PROFINET multiple-write dissection; protocol-specific feature. |
| !2203 | merged | GQUIC CGST decoding. Alexis La Goutte explicitly preferred more generic unknown-tag support; the later accepted !2248 parser design is the stronger precedent. |
| !2202 | merged | Makes local dissector variables static; internal-linkage cleanup. |
| !2201 | merged | **Deep / Guy Harris.** Removes fixed pcap/pcapng file-type/subtype constants in favor of runtime registration/accessors. Callers query the runtime identity and Export-PDU checks actual format capabilities before constructing metadata. |
| !2200 | merged | Stable-branch form of the Lua pcap/pcapng runtime subtype accessors; corroborates !2197/!2201. |
| !2199 | merged | MBIM signedness warning cleanup; no broader rule beyond type correctness. |
| !2198 | merged | Stable-branch form of Lua runtime subtype accessors. |
| !2197 | merged | Adds Lua accessors for runtime pcap/pcapng subtype identities; corroborates runtime registry design. |
| !2196 | merged | RTPS PID_DATA_REPRESENTATION refactor; protocol-local cleanup. |
| !2195 | merged | **Deep / Anders Broman.** Uses `proto_tree_add_item_ret_uint()` with the registered mask and correct little-endian decoding instead of manually fetching/masking. Output storage is widened to `guint32`, matching the helper contract. |
| !2194 | merged | ASAP statistics feature; no stronger general convention. |
| !2193 | merged | Developer guide recommends EditorConfig; style/tooling documentation. |
| !2192 | merged | **Deep / Guy Harris.** Fixes nested Wiretap block/option capability-table indexing: the option loop incremented/indexed the outer variable. Distinct `block_idx` and `option_idx` names make semantic index domains explicit. |
| !2191 | merged | RTCP padding/zero-length correctness fix; protocol-specific. |
| !2190 | merged | Replaces `atol` with GLib ASCII conversion helper in ZVT; portability/parse cleanup. |
| !2189 | merged | ZVT command-list dissection; protocol-specific. |
| !2188 | merged | **Deep / Anders Broman.** `tshark -G` exposed an out-of-order `value_string_ext` entry that forced linear-search fallback. Adding unassigned holes and restoring numerical order preserves indexed lookup. |
| !2187 | merged | **Deep / Guy Harris.** CMake generation of `wtap_modules.c` now depends on the actual `WIRETAP_MODULE_FILES` input set rather than the broader/wrong nongenerated-file list. |
| !2186 | merged | NetPerfMeter statistics feature and documentation; application-specific. |
| !2185 | closed/unmerged | Down-weighted. Sharkd invalid-request response attempt did not land and has no substantive review discussion. |
| !2184 | merged | **Deep / Gerald Combs.** New clang-tidy recursion warnings are a safety prompt: unbounded recursion should use `increment_dissection_depth()` / `decrement_dissection_depth()`; only proven-safe recursion should get a narrow `NOLINTNEXTLINE(misc-no-recursion)`. |
| !2183 | merged | **Deep / Guy Harris.** File handlers now advertise exact abstract block/option capabilities and multiplicity instead of coarse booleans. Wiretap may synthesize abstract blocks even if the native format has no literal block structure; interface-ID capability must mean packets can actually be associated with interfaces. |
| !2182 | merged | NetPerfMeter Windows compilation fix; portability maintenance. |
| !2181 | merged | Small FGP dissector improvement; protocol-specific. |
| !2180 | merged | URL corrections only. |
| !2179 | merged | Reassembly cleanup stops traversing once an already-visited fragment suffix is reached, avoiding repeated work in `free_all_reassembled_fragments()`. |
| !2178 | merged | **Deep / Anders Broman.** Rejects a local macro mini-language that hid ordinary tree additions, lengths, and offset movement; accepted code returns to explicit standard APIs. Windows CI also catches nonportable `u_int64_t` and narrowing, while an unrelated Qt warning is split to a separate MR. |
| !2177 | merged | Editcap help-output cleanup. |
| !2176 | merged | Automatic release-3.2 data/translation update. |
| !2175 | merged | Automatic release-3.4 data/translation update. |
| !2174 | merged | Automatic master data/translation update. |
| !2173 | merged | Spelling cleanup. |
| !2172 | merged | Sharkd redundant-declaration warning cleanup. |
| !2171 | merged | Makes more implementation-local variables/functions static. |
| !2170 | merged | Sharkd unused-parameter warning cleanup. |
| !2169 | merged | Raises macOS setup baseline to Qt 5.6/macOS 10.8; platform support policy. |
| !2168 | closed/unmerged | **Down-weighted architecture discussion / Guy Harris.** USER DLT is the wrong shortcut for NetMon 802.11 metadata; use a standard 802.11+metadata LINKTYPE such as radiotap/AVS or obtain a LINKTYPE assignment. Guy points to later merged !2599, where the NetMon dissector fully owns pseudo-header construction. |
| !2167 | merged | **Deep / Guy Harris + John Thacker caution.** Moves BER to runtime Wiretap registration and carries the pathname through record metadata rather than a separate setter. Post-merge Decode-As regression discussion shows the old dual-purpose syntax/OID table needs redesign; preserve the architectural lesson, not the regression. |
| !2166 | merged | Reassembly test debug-mode fix. |
| !2165 | merged | PTP power-profile support. Anders Broman insists the actual commit metadata/message satisfy project policy and asks for backward-compatibility evidence. |
| !2164 | merged | **Deep / Guy Harris.** Converts ERF/systemd-journal subtypes to runtime registration and documents a lifecycle requirement: libwiretap initialization must precede libwireshark/dissector registration when bindings depend on runtime subtype IDs. |
| !2163 | merged | Raises Qt minimum to 5.6; dependency baseline maintenance. |
| !2162 | merged | VJ compression expert-info API cleanup. |
| !2161 | merged | **Deep / Pascal Quantin.** A new shared DCCP service-code header is accepted only because other dissectors are intended consumers; otherwise the constants should remain in `packet-dccp.c`. The shared header must also be added to the epan build/public-header list. |

## Durable conclusions

- Runtime Wiretap file-type identities are registry state, not permanent compile-time integers (!2164, !2197–!2201).
- File handlers should declare fine-grained block/option capabilities; generic code should query them rather than infer them (!2183, corrected by !2192).
- Registered-field return helpers should own mask/endian decoding once, with correctly sized output storage (!2195).
- `value_string_ext` tables intended for indexed lookup must stay numerically ordered; `tshark -G` can expose fallback (!2188).
- Recursion warnings require a safety argument; depth helpers bound unbounded paths, with narrow suppression only after safety is established (!2184).
- CMake-generated outputs should depend on the exact source set consumed by the generator (!2187).
- Ordinary dissector code should favor explicit project-standard calls and visible offset progression over private macro DSLs that hide wire mechanics (!2178).
- Shared headers are justified by real cross-file consumers, not merely anticipated convenience (!2161).
- Closed !2168 is retained only as corroborating high-authority architecture history because later merged !2599 supplies the accepted pseudo-header ownership model.

No SMPTE ST 291/VANC packet type was encountered.
