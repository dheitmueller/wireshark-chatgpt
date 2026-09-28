# Durable conventions from Wireshark MRs 6211–6260

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## Strong conventions promoted in this run

### Bounds-check and qualify pseudo-headers before byte-order repair

Guy Harris's merged master !6257, with stable backports !6258 and !6259, first computes the effective packet size as the smaller of captured and reported lengths, verifies the SLL2 header is present, confirms the encapsulated protocol is CAN/CAN-FD, and only then swaps the CAN ID. The implementation uses a struct pointer solely to obtain member addresses/offsets; it never performs an alignment-sensitive typed dereference and uses a byte-wise swap.

Rule: capture post-processing must validate both available length and the semantic pseudo-header subtype before touching embedded fields. A native struct overlay is acceptable only if the code does not depend on its alignment/layout for typed accesses; prefer byte helpers or memcpy when actual loads are required.

### Do not strengthen an unspecified text encoding into a validation contract

John Thacker's merged !6244 keeps an 802.11 SSID as raw bytes when the standard leaves the character encoding unspecified. The raw bytes remain suitable for Dot11Decrypt, while the tree uses FT_BYTES with UTF-8-printable presentation. The code documents that true UTF-8 status can depend on later or previously observed Extended Capabilities state.

Rule: separate semantic bytes from presentation when the wire format does not guarantee a charset. Only decode as a validated string when the protocol provides enough context to justify that encoding.

### Prefer a local tolerant aggregation boundary over a global sentinel-contract change

Merged !6250 makes composite TVB append/prepend a no-op for NULL or zero-length members. Closed !6249 had proposed changing zero-length subset constructors to return NULL but was abandoned because that would require auditing all callers and because zero length can arise from several semantically different causes.

Rule: when optional/empty input can be harmlessly ignored at a narrow consumer boundary, prefer local tolerance over changing a foundational constructor's return contract across the tree unless every caller and semantic case has been audited.

### Initialize output buffers before any normal early return

Gerald Combs's merged !6223 makes proto_item_fill_label explicitly reject a NULL destination, then immediately writes the empty-string terminator before checking whether field information is available.

Rule: a caller-provided output buffer should contain a deterministic valid neutral value on every normal return path. Initialize it before branches that can return early; treat a NULL destination separately according to the API contract.

### Collect semantic facts first; format them later

Merged !6240 moves compile/runtime feature reporting from ad-hoc concatenation into a structured feature list. João Valverde repeatedly asked that mechanism changes remain separate from output-format policy and agreed that capability decisions should use direct semantic tests rather than parsing text generated for humans. Closed precursor !6225 is lower-weight history; merged !6240 is the accepted architecture.

Rule: keep capability/state discovery structured until the presentation boundary. Do not make program logic parse a human-readable summary of facts it could query directly. In refactors, separate the data model/mechanism from formatting-policy changes so compatibility can be reviewed independently.

### Keep generated external text from breaking generated source

In merged !6256, an automated ASTERIX refresh produced invalid C because quotation marks from external text were emitted without escaping. Gerald Combs manually reverted that generated artifact from the weekly refresh; the generator-side escaping fix was handled separately in !6262.

Rule: generated-source pipelines must escape external text for the target language before emission and must validate that regenerated source compiles/parses. If a routine data refresh generates syntactically invalid source, omit/revert that artifact until the generator is fixed rather than committing broken generated output.

### Keep checker CLI scope semantics coherent across the tool family

Merged !6260 changes five checker scripts so --file can be supplied repeatedly, applies the same path normalization rules to each selected file, and fails explicitly when a selected path does not exist. Merged !6230 independently improves the spelling checker by recognizing a reusable number-plus-unit pattern instead of extending a dictionary with every numeric combination.

Rule: sibling checker tools should expose consistent selection semantics and deterministic validation. When a recurring lexical class can be described reliably, encode the pattern rather than growing a brittle exception list.

### Encoding arguments should express semantics, not redundant flags

Merged !6220 removes ENC_NA from ASCII string-item calls across the tree and updates the fixer/checker accordingly: FT_STRING/FT_STRINGZ text has a character encoding but no integer byte-order semantics.

Rule: pass only encoding flags meaningful for the registered field type and operation. Do not retain redundant byte-order/no-byte-order flags on plain strings merely for historical consistency.

## Secondary useful evidence

!6238 (John Thacker) fixes a 32-bit Windows build by replacing pointer-difference loop bounds with direct in-buffer pointer comparison. !6245 fixes a bitset membership test that mixed a size constant with a discriminator enum. !6253 keeps display-filter diagnostics safe when the offending input is non-printable. !6224/!6227 show that derived fingerprints such as JA3 must apply their specification's canonical exclusions (GREASE). Closed !6226 suggests preserving unknown trailing bytes in forward-extensible protocol structures, but it is intentionally not promoted as accepted implementation precedent.
