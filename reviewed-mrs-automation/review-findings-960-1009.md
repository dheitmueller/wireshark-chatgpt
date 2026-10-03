# Review findings: !960-!1009

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

This run reviewed exactly 50 MRs, !1009 through !960. Merged master work and explicit maintainer review were treated as the strongest evidence. Stable-branch backports mainly corroborate the corresponding master behavior. Closed !1005, !990, and !977 are retained for accounting and useful discussion, but their proposed implementations are not treated as accepted precedent.

## High-value durable findings

### !975 (with closed !1005): typed-item checker findings require protocol-semantic validation

Martin Mathieson's merged !975 fixes several fields whose registered integer widths were smaller than the byte lengths passed to `proto_tree_add_item()`. In review he explicitly described checking the protocol specification before changing a field, including an A21 field whose current value table used only one byte while the specification defined a two-byte field. He also posted the current whole-tree output from `tools/check_typed_item_calls.py` rather than treating the three corrected instances as proof that the class of issue was exhausted.

Closed !1005 supplies weaker corroboration: Martin questioned whether a MySQL field used with four-byte call sites should be widened rather than hidden behind a checker exception, while noting that legitimate shared-use patterns need careful modeling.

**Durable rule:** typed-item checker diagnostics are structural leads, not automatic edits. Audit all uses, check the wire specification, fix the semantic type/width where appropriate, and rerun the checker across the relevant change set. A warning disappearing is not evidence that the resulting field definition is protocol-correct.

### !1001: reuse the protocol layer that owns framing and reassembly

The merged OBD-II change moves OBD-II under the existing ISO 15765-2/ISO-TP dissector instead of partially implementing ISO-TP behavior inside OBD-II. Guy Harris explicitly asked whether the referenced 15765 layer was ISO 15765-2; the author confirmed that OBD-II specifies it as the transport/network layer. The resulting composition lets the mature ISO-TP layer perform multi-packet reassembly and pass the completed payload to OBD-II.

**Durable rule:** model the actual protocol stack. If framing, segmentation, or reassembly is owned by an existing parent protocol, make the upper protocol its subdissector rather than duplicating a partial transport implementation in the child.

### !971 (with !981/!982): expose semantic operations instead of abusing accessors for side effects

Guy Harris found that packet-list code called `columnString(..., 1, true)` and discarded the returned string merely to trigger colorization. Because column indices are zero-based and a second column is not guaranteed to exist, the shortcut crashed with a one-column configuration. After discussion with Gerald Combs, Guy added the semantic `ensureColorized()` operation and the stable branches carried the same fix.

**Durable rule:** if callers need an operation, expose that operation directly. Do not call an unrelated getter/accessor solely for a hidden side effect and thereby inherit arbitrary preconditions of the value being fetched.

### !970: name dissectors for the wire protocol, not the application using it

The new Mosh dissector was initially named for the application. Jaap Keuter pointed out that Wireshark dissectors represent protocols: applications such as PuTTY can speak Telnet or SSH without those dissectors being named after the application. The accepted implementation was renamed to SSyncP, with a disambiguated abbreviation because `ssp` was already occupied.

**Durable rule:** protocol/file/filter identity should describe the wire protocol being dissected. When an obvious abbreviation collides with an existing protocol, choose a distinct protocol abbreviation rather than substituting an application/product name.

### !968: one-bit boolean fields should be tested by truth value, not equality with TRUE/FALSE

Peter Wu noted that one-bit boolean bitfields may have a numeric true representation that is not the literal GLib `TRUE` value (for example, a signed one-bit field can read as -1). The accepted QUIC loss-bit code uses ordinary truth tests instead of `x == TRUE` / `x == FALSE`.

**Durable rule:** for C boolean/bitfield state, test `x` or `!x` unless an API contract explicitly requires comparison to a named non-boolean status value. Do not assume every true representation numerically equals 1.

### !984 (with !986): shared dissector context must carry downstream semantic state

Nordic BLE decoded the PHY into its capture-specific context but failed to copy it into the common Bluetooth context consumed later. LE Coded PHY packets therefore omitted the coding indicator and parsed subsequent bytes at the wrong offset. The merged master fix copies the PHY into the common context; !986 backports it.

**Durable rule:** when a parent/capture-specific layer computes state required by a shared downstream dissector, propagate that semantic state through the common context explicitly. A value being correct in one private context does not make it visible to the next layer.

### !961: initialize every member of shared context structures on every construction path

OSS-Fuzz exposed a null/wild-pointer failure in Bluetooth because two lifecycle pointers in `acl_data` were not initialized on BTLE construction paths. The merged fix initializes both before handing the context downstream. Gerald Combs noted that zero-initialized wmem allocation could also make this class of omission harder to introduce.

**Durable rule:** treat a dissector-data/context struct as an interface contract. Every producer must initialize every member that a consumer may inspect. Prefer zero-initialized allocation when zero is a valid sentinel, but still set nonzero/default semantics explicitly.

### !974: optional-feature guards must cover the complete feature-only dependency graph

The QUIC no-AEAD build failed because helper functions used only from `HAVE_LIBGCRYPT_AEAD` code remained outside the feature guard. Ivan Nardi caught the guard boundary in review; Pascal Quantin moved it to cover the helpers as well as their consumers.

**Durable rule:** optional-feature builds are first-class configurations. Guard feature-only helpers, state, and call sites consistently and exercise a build with the feature disabled; a successful default build is not sufficient.

### !980: recognition paths must prove minimum captured length before probing fields

RPC-over-RDMA now checks for `MIN_RPCRDMA_HDR_SZ` before reading fields used to decide whether a packet belongs to the protocol. The source comment explicitly states that the probing `tvb_get_ntohl()` must not throw while checking whether this is an RPCoRDMA packet.

**Durable rule:** a recognition/heuristic path must first prove that every byte needed for the discriminator is captured. Nonmatching or truncated traffic should cause a clean decline rather than an exception.

### !979: combine fetch and tree insertion when Wireshark provides a return-value add-item API

During R-GOOSE review, Anders Broman requested using `proto_tree_add_item_ret[u]int()` rather than separately fetching a value and adding it to the tree. The accepted implementation uses the combined APIs where appropriate.

**Durable rule:** when the same wire field must both be displayed and consumed by code, prefer the typed `proto_tree_add_item_ret_*()` family where it matches the field semantics. This keeps bounds/encoding/tree extraction on one API path and avoids duplicate reads.

## Additional corroborating evidence

- !999 adds Protobuf/gRPC regression coverage with representative pcaps, protobuf schema fixtures, Lua subdissector integration, and targeted pytest invocation. Configuration-driven dissectors should bring the configuration/schema material needed to make tests reproducible.
- !960 with stable backports !969/!976 fixes RFC2190 masks; the contributor notes that comparing the field definitions with the packet diagram made the mask errors obvious. Protocol diagrams/spec layout are useful independent checks for bitmask metadata.
- !972 fixes end-of-element RDM strings by using the bytes remaining after preceding fixed fields, rather than reusing the enclosing element's original total length.
- !997, authored and merged by Guy Harris, factors repeated Windows blocking-pipe read mechanics into a shared helper and documents the socket-vs-pipe platform distinction. Repeated platform-specific I/O sequences should have one semantic implementation.
- !977 contains useful Guy Harris/Pascal Quantin/Jaap Keuter discussion about delaying pcapng capture-file readiness notification until an SHB has actually been written, but the MR closed unmerged while the broader pipe path was still under review. Treat the design discussion as context, not accepted implementation precedent.
- !990 similarly contains useful Jaap Keuter reasoning that `TvbRange.raw()` must honor the range object's own bounds and API semantics, but it closed because the issue had already been resolved elsewhere; do not treat that patch itself as the accepted fix.
- !983 reiterates submission readiness: enable maintainer modification and remove Draft/WIP state when the change is actually ready for review.
- !961 independently reiterates Wireshark commit-message requirements through Graham Bloice review.
