# Durable conventions from !2611–!2660

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Treat format version numbers as compatibility contracts, not feature inventories

Merged !2631 received extensive Guy Harris review after Sysdig-generated pcapng files used SHB version 1.2 while adding new block types. Guy pointed out that pcapng's minor version is for an incompatible change that an older reader cannot safely consume, while adding a new block type or option is specifically not such a change. After confirming that the newer Sysdig material was ignorable by readers that did not know those blocks, Guy updated the specification/libpcap behavior and the Wireshark work accepted 1.2 as equivalent to 1.0 for reading. Guy-authored !2646, with release backports !2649 and !2651, then made that rationale explicit in the reader.

**Rule:** a container-format version field describes compatibility semantics, not a list of features present. Do not bump it merely because a new ignorable block/option exists. Readers may deliberately tolerate a known deployed noncanonical version when its wire semantics are compatible, while writers should continue to emit the canonical version.

## Select a record layout before validating and parsing it

The accepted Sysdig event-v2 implementation in !2631 chooses the minimum event size from the block type, validates the total block length against that minimum, and then derives remaining payload bytes from the selected layout. Guy Harris explicitly steered the implementation toward this structure.

**Rule:** where a record family has multiple wire layouts, identify the layout first, derive one authoritative minimum/header size, reject undersized input before field reads, and use that same size for subsequent remaining-length accounting.

## Keep signedness consistent across the complete field API path

Merged !2625 changed an RTPS locator port from signed to unsigned based on the protocol specification. That required changing the registered field from `FT_INT32` to `FT_UINT32`, the destination variable to `guint32`, the tree helper to `proto_tree_add_item_ret_uint()`, and user-facing formatting to `%u`. `tools/check_typed_item_calls.py` then found a remaining incompatible `_ret_int()` call, fixed in !2629, and !2630 finished the remaining local variable/formatter mismatch.

**Rule:** a signedness correction is not complete when only the hf registration changes. Audit every extraction helper, destination C type, formatter, comparison, and caller. Run the typed-item checker after the change to catch stale call sites.

## Prefix even translation-unit-local dissector helpers with the protocol identity

During merged !2640, Anders Broman requested `tiff_*` names for internal variables/helpers and the usual `dissect_tiff_*` spelling for dissection functions even though they were static.

**Rule:** `static` linkage does not eliminate the value of protocol-scoped naming. Use the dissector/protocol abbreviation consistently for internal helpers and data so large source files and review/search results remain unambiguous.

## A worker thread's shutdown is part of the owning object's lifetime contract

Merged !2616 moves capture-filter syntax checking to a QObject worker running in a QThread event loop and replaces the old endless condition-variable loop with queued signal/slot work. The owner destructor calls `quit()` and `wait()` before destroying the thread and worker.

**Rule:** an owning UI object must not destroy a worker or its thread while queued work can still execute. Make shutdown explicit: stop the event loop, join the worker thread, then release the participating objects.

## Static-analysis cleanup must preserve semantic state changes

In merged !2613, Pascal Quantin reviews cppcheck-driven NAS-5GS changes and requires protocol-significant operations to remain while allowing genuinely redundant initializers to be removed.

**Rule:** treat analyzer warnings as prompts to prove liveness/semantics, not authorization to delete code mechanically. In parsers, assignments, offset changes, and state transitions can be required even when a tool regards a surrounding value as redundant.

## Preserve the distinction between a valid partial representation and an uninitialized/null sentinel

Merged !2626 fixes comparisons involving `FT_PROTOCOL` values. John Thacker explains that a newly allocated all-NULL value can remain in the type's null/uninitialized equivalence class and should not be compared, while a valid literal value that legitimately lacks a protocol description still needs a safe non-NULL string component so comparisons with another partial representation cannot dereference NULL.

**Rule:** for compound value types, define invariants for each valid constructed representation separately from the sentinel/uninitialized state. Repair constructors that produce comparable values rather than globally erasing a meaningful NULL state.

## Keep vendored-source fixes on an upstream path

Merged !2660 modifies QCustomPlot for analyzer findings, and Anders Broman immediately asks whether the fixes were reported upstream. This independently corroborates the earlier vendor-source guidance.

## Keep protocol MRs focused and capture-backed

Closed !2638 mixed several PTP changes, including pieces already present elsewhere, plus unrelated formatting churn. Anders Broman requested history cleanup and less gratuitous change, while Lars Völker identified the mixed scope and asked for example files. The MR was closed after useful pieces landed elsewhere.

**Rule:** keep a protocol MR centered on one coherent behavior change, avoid unrelated reformatting or already-implemented work, and provide representative captures when the change needs traffic to validate it.

## Generated dissector inputs remain authoritative

Merged !2655–!2657 update S1AP/X2AP/NGAP by changing ASN.1, configuration/template inputs, and generated packet dissectors together.
