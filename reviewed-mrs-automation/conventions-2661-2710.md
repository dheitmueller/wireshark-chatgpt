# Durable conventions from !2661–!2710

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

## Generalize metadata-versus-payload separation at the right layer
Merged !2661 adds a separate Packet Bytes data source for the over-the-air IEEE 802.15.4 payload inside a TAP capture. Guy Harris accepts it as a useful short-term prototype but stresses that metadata-prefixed link types are a general problem, not an IEEE 802.15.4 special case. He points toward a common metadata representation usable across encapsulations and potentially split appropriately between libwiretap and libwireshark, so non-dissecting tools such as editcap can translate capture metadata too.

**Rule:** when the same metadata/payload separation problem appears in multiple link-layer formats, avoid making the permanent architecture a pile of per-dissector exceptions. Consider whether normalized capture metadata belongs in Wiretap, with libwireshark consuming that representation for presentation.

## Keep application-specific CLI semantics out of generic option parsers
Merged !2692, authored by Guy Harris, removes `-k` from `capture_opts_add_opt()`. Starting capture immediately is a Wireshark GUI semantic; dumpcap always captures and TShark's start behavior follows interface selection. Shared option infrastructure should contain only options whose meaning is genuinely shared.

## Explicit dispatch before heuristics
Merged !2681 fixes MQTT payload dispatch so UAT-configured and media-type dissectors run first; heuristics are attempted only when neither path actually handles the payload. The implementation uses the dispatch return value to track success.

**Rule:** order subdissector selection from strongest explicit evidence to weakest heuristic evidence, and use the called API's handled/not-handled result rather than assuming invocation means success.

## Do not cast away const for an unknown subdissector
Merged !2670 initially faced pressure to cast away `const` from an MQTT topic string passed via the generic dissector data pointer. The accepted code copies the string into packet-scope writable storage before exposing it to heuristic subdissectors.

**Rule:** a generic `void *data` callback boundary does not justify granting mutable access to caller-owned read-only memory. If the callback contract is mutable and cannot be changed, provide appropriately scoped mutable storage.

## Validate packaged runtime data from installed artifacts
Merged !2706 adds SparkplugB together with a bundled protobuf schema and packaging changes. The review explicitly tests the generated macOS installer on a separate system; running from the build directory was not considered sufficient to validate installation of the schema. Cross-platform compilation also caught a last-minute typo.

**Rule:** when a feature depends on installed data files, schemas, plugins, or other packaging payloads, test an installed package on the relevant platform. Build-tree success does not prove that packaging manifests and runtime search paths are correct.

## Treat vendor-source edits as upstream candidates
In merged !2707, Anders Broman asks whether the QCustomPlot performance fix was pushed upstream as well. Local fixes to vendored third-party code increase divergence and future merge burden.

**Rule:** when modifying bundled third-party code rather than Wireshark-owned integration code, pursue the fix upstream when practical and keep the local patch aligned with that upstreaming path.

## Fix checker findings at the semantic source
During merged !2699, Guy Harris rejects resolving a Doxygen warning by deleting a meaningful directive. The accepted change corrects the actual mistaken parameter name while retaining the documentation structure.

**Rule:** static/documentation checks are not satisfied merely by making the warning disappear. Preserve intended semantics and correct the source of the mismatch.

## Generated-source ownership remains authoritative
Merged !2693 fixes the sysdig const type in both the generated dissector and `tools/generate-sysdig-event.py`; !2663–!2666 likewise keep ASN.1/config/template inputs synchronized with generated C. Fix generator inputs and regenerate checked-in outputs.
