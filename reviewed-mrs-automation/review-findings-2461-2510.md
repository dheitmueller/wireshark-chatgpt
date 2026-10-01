# Review findings 2461-2510

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

The exact reviewed-set ledger is in `ledger-2461-2510.md`. This batch contains 48 merged MRs and two closed/unmerged MRs (2494 and 2473). Merged master changes and direct maintainer guidance are weighted most heavily.

## Highest-value findings

### 2501, 2491, 2495, 2498, 2502, 2503 — Large File Support configuration

Guy Harris authored the master LFS replacement and follow-up ordering fix, with stable backports. The original CMake path did not actually supply needed flags on 32-bit Ubuntu. The accepted replacement reuses libpcap's proven detection logic, and 2501 moves the checks ahead of all source subdirectories so definitions apply everywhere. João Valverde also caught obsolete helper files left behind by the replacement; Anders Broman surfaced a stale generated-build dependency on a deleted CMake input.

### 2479, 2465, 2490 — display-filter regexes are not protocol field types

Guy Harris authored the sequence that removes FT_PCRE from ftypes. Regex values move into the display-filter syntax tree/VM, with a dedicated VM constant representation and matching path. The field registry is left for values that can actually appear as protocol fields.

### 2466 and 2467 — assertion/error choice depends on execution boundary

Guy Harris changed frame/TCP dissection from ordinary GLib assertions to dissector assertions so a violated dissector invariant is reported through dissection rather than taking down the application. In preference registration, by contrast, duplicate/structurally impossible module registration becomes an explicit g_error with a useful message because the global registration state is invalid.

### 2468 — registration is the plugin opt-in

Anders Broman challenged a protobuf preference that would additionally authorize subdissectors on non-string/non-bytes fields. The merged revision removes the extra gate and invokes the precise keyed registration for any named protobuf field.

### 2482 — broad USB class registration caused false decoding

IPPUSB had registered itself for unknown and vendor-specific USB classes, causing unrelated devices such as CP210x bridges to be decoded as IPPUSB and reported malformed. The merged fix keeps only the printer-class registration. Pascal Quantin explicitly asked about Decode As as the user override path.

### 2470 — capture subsystem ownership

João Valverde merged caputils and capchild under capture. Guy Harris's review gives the architectural context: some code wraps libpcap/WinPcap/Npcap platform/version differences, while other code implements capture functions those libraries do not provide. He called out capture_ifinfo as an interface imported by several independent consumers without one clear owner and suggested the abstraction deserved redesign; capture_opts was mentioned similarly.

### 2461 — wrap-aware TCP sequence arithmetic

John Thacker fixed multisegment PDU membership/length logic by replacing ordinary integer comparisons with LE_SEQ/GT_SEQ and by taking the minimum after converting endpoints to distances from the current sequence number. The MR explicitly notes that subtraction cannot be distributed across MIN when wraparound is possible.

## Other useful merged evidence

- 2497 contains direct Pascal Quantin review that nested calls do not justify a pointer-to-pointer when a single pointer is sufficient. The author kept the already-merged/backport flow and landed the simplification as follow-up 2508 so branches could preserve coherent commit history.
- 2496, authored by Guy Harris, again confirms that guint64 is not portably unsigned long and must use GLib's 64-bit format macros.
- 2500 keeps the LDAP ASN.1 template and generated packet-ldap.c synchronized while correcting the SASL subtree byte range.
- 2483 through 2489 keep ASN.1 cnf inputs synchronized with generated dissectors and normalize manual filter overrides to the generator's underscore convention.
- 2486 keeps the small FIND protocol inside packet-arinc615a.c after the author explained it is defined as part of that tightly coupled ARINC 615A context; Alexis La Goutte had asked whether it deserved its own file.
- 2463 rewrites wmem_strbuf_append_vprintf around vsnprintf and adds/updates tests. The author notes that max-length truncation of multibyte strings remains an existing limitation rather than silently claiming the rewrite fixes it.
- 2506 removes duplicate USB HID setup dissection once the non-standard request has already been handled.
- 2492 introduces extended RTP sequence/timestamp state so later code can reason across wrap without repeatedly reconstructing it.

## Lower-weight closed MRs

### 2494 — reassembly performance experiment

The WIP changed fragment lists to doubly linked form and cached contiguous length. Pascal Quantin suggested a next/first-hole pointer as a cheaper optimization. Tomasz Mon requested that temporary sanity checks be turned into automated reassemble_test.c coverage. John Thacker suggested separating fragment_head and fragment_item to avoid paying head-only fields per fragment. Later discussion records that the first-gap optimization delivered major gains without the doubly linked list. Because the MR closed unmerged, use it as design-history evidence rather than an implementation exemplar.

### 2473 — forcing TShark columns with a CLI switch

The proposal added --columns so Lua/post-dissectors could see packet-list columns. Guy Harris instead asked whether consumers should declare that they require columns at registration time, analogous to tap flags, making the behavior automatic. He also pushed the discussion toward making _ws.col fields usable consistently in packet-matching expressions. The author closed the MR and moved the discussion to an issue.
