# Durable conventions from Wireshark MRs !6361-!6410

Corpus commit: ddcaa22b51c68f594e425a23388c3a2086813054

## pcap compound linktype metadata

Merged !6362 and !6364 are both authored by Guy Harris and therefore carry unusually high architectural weight. The pcap header's 32-bit network field is not one opaque linktype number. The low 16 bits carry the link-layer type so it remains compatible with pcapng's 16-bit linktype field. Other bits have independent semantics: an FCS-length-present flag gates whether the encoded FCS length is meaningful, while reserved bits must be validated as reserved.

!6362 stores the decoded FCS length in reader state and passes it into normal record post-processing instead of discarding it. !6364 corrects an earlier interpretation of reserved bits as a class namespace and rejects nonzero reserved values.

**Rules:** decode compound capture metadata according to the defined bitfield contract; never interpret a value field unless its validity/presence flag says it is present; propagate decoded metadata through both reader state and packet post-processing; and do not turn historically proposed/reserved bits into active semantics without specification authority.

## Direction-sensitive protocol state

Merged !6378 fixes Bluetooth GATT state when both peers expose services. Direction becomes part of request and handle-database identity. John Thacker then catches a deeper issue: Mesh Proxy Data In and Mesh Provisioning Data In are duplex cases whose semantic service direction is opposite the current packet direction, so using pinfo direction mechanically is still wrong.

**Rule:** when protocol state belongs independently to each direction, include direction in the state key. Derive that direction from the protocol operation's semantics, not merely from the current packet's source/destination orientation. Test request/response pairs and duplex message classes whose lookup direction differs from packet direction.

## Use typed capture-option representations after validation

Merged !6380 stops re-decoding frame verdict options from a generic byte array and consumes the typed packet_verdict_opt_t representation produced by the option layer. TC/XDP verdicts are read from typed integer members and byte-oriented verdicts from their typed byte container.

**Rule:** once a lower capture/parsing layer has validated and normalized an option into a typed representation, downstream dissectors should consume that representation instead of repeating raw-byte interpretation. Duplicating the wire decode creates inconsistent validation, length assumptions, and crash paths.

## Evolve display-filter syntax with compatibility and semantic clarity

Merged !6398 keeps the old ~= spelling accepted while marking it deprecated, emits a diagnostic naming the replacement !==, and updates documentation and release notes. The discussion explicitly acknowledges that several releases exposed different spellings and uses a transition period to reduce disruption.

Closed !6367 is useful negative evidence only: João Valverde proposed =~ as a contains alias, then agreed it was misleading because that spelling conventionally suggests regular-expression matching.

**Rules:** when replacing user-facing filter syntax, keep a compatibility window when practical, emit a precise migration diagnostic, and document the change. Do not adopt operator spellings whose conventional meaning conflicts with the actual semantics merely to provide a symbolic alias.

## Respect dependency API floors and avoid platform assumptions

During merged !6385, John Thacker points out that g_string_replace requires GLib 2.68, above Wireshark's then-supported minimum and unavailable on platforms such as RHEL 8 and Debian Bullseye. A feature or cleanup that compiles on the contributor's machine is not acceptable if it silently raises the dependency floor; use a compatibility helper or older supported API unless the project deliberately raises the minimum.

Guy Harris also corrects an assumption that libpcap's "version" wording is Windows-specific: pcap_get_lib_version() on UNIX-like platforms and Haiku includes it as well.

**Rules:** check newly used dependency APIs against the declared minimum version, not the local version. Before moving generic behavior into platform-specific code, verify the external API's behavior across supported platforms rather than inferring scope from one implementation.

## Keep disabled specialized targets explicitly covered by CI

Merged !6407 disables fuzzshark by default, including on non-Windows platforms, but explicitly enables BUILD_fuzzshark in a CI configuration that is intended to build it.

**Rule:** disabling an expensive or specialized target in the default developer build must not accidentally remove compile coverage. Choose an appropriate CI job and enable the target there explicitly.

## Representative captures remain part of protocol review

In merged !6401 Ivan Nardi asks for a QUIC CIBIR trace and Alexis La Goutte supplies it. In merged !6395 Ivan independently verifies the draft-34 decryption fix and attaches the capture that reproduces the old failure.

**Rule:** protocol additions and decode/decrypt fixes should carry a focused capture when redistribution permits, ideally one that demonstrates the exact changed path and can serve as a future regression fixture.

## Cross-source consistency checks can complement normal document builds

Merged !6369 adds a small checker that compares help URLs embedded in UI code with anchors present in the User's Guide source. Martin Mathieson distinguishes this from ordinary documentation-link checking, and the script exits nonzero when application references have no matching guide target.

**Rule:** if an invariant spans two source domains that their normal build steps do not jointly validate, a focused repository checker is appropriate. Drive it from the authoritative source forms and make missing relationships fail explicitly.

## Treat historical architecture as superseded when later merged work refines it

Merged !6406 introduces an initial register_log_conversation_filter API but routes packet and log registrations into the same underlying list. Later merged !6646, already reviewed in the notebook, separates packet and log conversation-filter registries because they are different semantic domains.

**Rule:** the merge bit is strong evidence for what was accepted at that time, but later merged corrective/refactoring work is stronger evidence for current convention. Do not promote an early transitional architecture over its accepted successor.
