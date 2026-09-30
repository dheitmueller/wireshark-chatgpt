# Review findings: !4061–!4110

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`

Merged MRs are weighted as implementation precedent. Closed/superseded MRs are used only for negative-design guidance, review history, or rationale.

| MR | State | Finding |
|---|---|---|
| !4110 | Merged | Alexis La Goutte: Clang Analyzer dead-store cleanup across dissectors, wiretap, extcap, and proto; meaningless assignments removed, while useful error values are consumed in diagnostics. |
| !4109 | Merged | Corrects conflicting/incorrect registered field abbreviations in spoolss, Extreme EXEH, and NCSI. |
| !4108 | Merged | Jaap Keuter: master-3.2 backport of 3GPP 29.234 Diameter dictionary update. |
| !4107 | Merged | Jaap Keuter: release-3.4 backport of the same Diameter update. |
| !4106 | Closed | Gerald Combs abandoned an automatic update after a shallow-clone experiment clobbered AUTHORS; lower-weight automation/provenance caution. |
| !4105 | Merged | Release-3.4 backport of IEEE 802.15.4 PAN-ID-present handling. |
| !4104 | Merged | Thrift partial-schema support. Jaap Keuter rejected inferring fallback from wire field ordering; Anders Broman proposed explicit `DE_THRIFT_T_GENERIC`, which the merged implementation uses for intentional per-field generic dissection. |
| !4103 | Merged | Master fix honoring the IEEE 802.15.4 PAN-ID-present bit. |
| !4102 | Merged | Master 3GPP 29.234 Diameter dictionary update. |
| !4101 | Merged | Exports `epan_set_always_visible()` for C plugins. Roland Knall stresses the performance cost of all-fields-visible and favors lifecycle-scoped use around tap creation/removal and redissection. |
| !4100 | Merged | NSIS presentation fix for long build/version titles. |
| !4099 | Merged | CI preserves human-readable and XML cppcheck output as artifacts. |
| !4098 | Merged | SparkplugB malformed-topic hardening checks missing topic components before generated-field insertion. |
| !4097 | Merged | Spelling cleanup also renames registered filters; historical counterexample only because stronger later review treats filter names as compatibility surfaces. |
| !4096 | Merged | John Thacker AVTP-over-UDP support. Anders Broman challenged broadening default port claims despite IANA registration; final MR removed the proposed second default port while retaining the protocol fix. |
| !4095 | Merged | O-RAN section-summary range/off-by-one display correction. |
| !4094 | Merged | Display-filter conflict fix. Martin Mathieson demonstrates `check_typed_item_calls.py --consecutive` and explains that CI hard-fails only clear checker errors while warning-only output still merits semantic review. |
| !4093 | Merged | BGP-LS IGP-TE metric offsets fixed and TLV type/length fields displayed. |
| !4092 | Merged | Guy Harris removes a stale portability comment after its GLib size assumption no longer matches reality. |
| !4091 | Merged | CI artifact added for header-field conflict diagnostics. |
| !4090 | Merged | UDP length-field blurb clarifies that length includes header and data. |
| !4089 | Merged | Pointer initialization added for maybe-uninitialized analysis. |
| !4088 | Merged | O-RAN decompression replaces an undefined shift at exponent zero with arithmetic defined across the accepted domain. |
| !4087 | Merged | IEEE 1722 terminology corrected from historical AVBTP naming to AVTP. |
| !4086 | Merged | Residual LZ4 conditional-build fix after overlapping work was superseded by adjacent MRs. |
| !4085 | Merged | Guy Harris redesigns Wiretap buffer size/offset types around backend limits: buffers capped at 2^30 and represented with `guint` because MSVC `_read()` and zlib expose narrower integer contracts. |
| !4084 | Merged | Doxygen parameter warning cleanup. |
| !4083 | Merged | AVTP compressed-video handling delegates MJPEG/H.264 to existing dissectors, respects declared payload extent, and treats reserved formats as data with expert indication. |
| !4082 | Closed | Gerald Combs cast-only Wiretap fix superseded by !4085; Guy Harris review reinforces the external-I/O integer-range rationale. |
| !4081 | Closed | Broader buffer redesign superseded by the simpler accepted path; Guy explicitly rejects assuming pointer-sized `gsize/gssize` are automatically correct for `_read()`-bounded I/O. |
| !4080 | Merged | Debian/macOS build corrections for the compressed-reader implementation. |
| !4079 | Merged | F5 conversation filters now require the F5 trailer protocol to actually be present. |
| !4078 | Merged | USB HID extended-usage support replaces an arbitrary report-descriptor count cap with protocol-domain constraints; review clarifies extended usages are local overrides, not global usage-page mutation. |
| !4077 | Merged | ASN.1 include fix applied to the authoritative template and regenerated output. |
| !4076 | Merged | Notarization failure diagnostic points to Apple's service-status source. |
| !4075 | Merged | O-RAN preference removal retains `prefs_register_obsolete_preference()`; Jaap Keuter treats persisted keys as compatibility state even though the preference existed only on development master. |
| !4074 | Merged | CMake test path is derived from `TARGET_FILE_DIR` for the actual `wmem_test` target, not another executable whose layout differs on some macOS configurations. |
| !4073 | Merged | Crypt API documentation corrections prompted by `-Wdocumentation`. |
| !4072 | Merged | GSM MAP version-specific ResetArg support implemented in ASN.1/template sources and regenerated output. |
| !4071 | Merged | RTCP child parser clears parent padding state after consuming padding, preventing outer framing from consuming the same bytes again. |
| !4070 | Merged | Thrift pointer initialization for maybe-uninitialized analysis. |
| !4069 | Merged | RTPS alignment correction removes an incorrect alignment-zero reset. |
| !4068 | Merged | IEEE1905 DPP category/length correction for encapsulated frames. |
| !4067 | Merged | 802.11 CCMP AAD construction uses the QoS-specific FC mask so HT-Control presence does not corrupt authentication data. |
| !4066 | Merged | Vector BLF WLAN support. Alexis La Goutte requested a sample capture; contributor supplied a short BLF. Reader validates object/header lengths before copying packet data. |
| !4065 | Merged | Aruba IAP device-ID table addition. |
| !4064 | Merged | `validate-clang-check.sh` option handling and analyzer artifact output corrected. |
| !4063 | Merged | ITS custom value formatting updated in ASN.1 configuration/template-generated code. |
| !4062 | Merged | Gerald Combs removes handcrafted `WS_PROGRAM_PATH` and derives executable locations with CMake `TARGET_FILE_DIR` expressions. |
| !4061 | Merged | NAS-5GS port-management information container corrected to the extended-length TLV form. |
