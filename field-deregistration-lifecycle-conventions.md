# Wireshark Field Deregistration Lifecycle Conventions

This file records durable lifecycle guidance for dynamically deregistered protocol fields. Current upstream source remains authoritative.

## Do not free deregistered fields from dissectors or preference callbacks

Calling `proto_deregister_field()` does not make the corresponding field metadata immediately safe to free. Other Wireshark state may still hold references to the field, including while a dissector preference callback is running. `proto_free_deregistered_fields()` is therefore an owning-framework lifecycle operation and must not be called by dissectors, including from preference callbacks; the framework will invoke it when references are no longer live.

When dynamically allocated companion data must have the same lifetime as deregistered fields—for example, an array containing `hf_id` pointers—register that allocation with `proto_add_deregistered_data()` instead of trying to force immediate field destruction.

**Evidence:** merged master MR !19260, authored and merged by John Thacker, documents this contract directly in `proto.h` after issue #20425 exposed the unsafe lifetime assumption.

**Implementation rule:** distinguish deregistration from destruction. A dissector may remove a field from future registration use at the supported lifecycle point, but ownership of the underlying registered-field objects remains with epan until the framework reaches its safe reclamation phase. Tie related dynamically allocated metadata to that deferred reclamation mechanism rather than freeing either side early.

**Review implication:** preference callbacks are not a safe teardown boundary merely because they are where a dynamic field set changes. When reviewing code that rebuilds dynamic `hf` arrays, look for immediate calls to global/freeing registration APIs and verify that stale references cannot survive the callback.

**Confidence:** Extremely high. The rule is stated directly in a merged API documentation change authored and merged by John Thacker, with the unsafe caller class and supported replacement mechanism named explicitly.

## Give dynamically deregistered field sets an exclusive namespace

When bulk deregistration identifies dynamic fields by display-filter prefix, the prefix must describe an exclusive ownership boundary for that dynamic field set. Do not use a prefix that can also match static fields or fields owned by another subsystem: bulk removal must never deregister unrelated fields.

A namespace change can be compatibility-sensitive because display-filter abbreviations are user-visible API. However, merged MR !18907 demonstrates that preserving an unsafe/shared prefix is not preferable when it prevents correct ownership selection. The Signal PDU dynamic fields were moved from `signal_pdu.*` into the narrower `signal_pdu.signals.*` namespace so the new linear-time bulk deregistration path could select only dynamic signal fields. The discussion also records that overlapping dynamically generated filter names could overwrite existing filters and lead to crashes; the same concern applied to SOME/IP.

**Implementation rule:** design the filter namespace at registration time so lifecycle ownership can be expressed without scanning or guessing later. If a dynamic family needs to be rebuilt or deregistered as a unit, give it a prefix not shared with longer-lived fields.

**Review implication:** when a dynamic-field optimization introduces prefix-based registration/deregistration, review both sides of the contract: (1) whether the prefix exclusively identifies the intended ownership set, and (2) whether changing existing filter abbreviations creates a compatibility cost that should be documented or mitigated. Correctness and ownership isolation take precedence when the old namespace is unsafe.

**Evidence:** merged master MR !18907. Lars Völker reported roughly a 10x profile-switch improvement for large configurations (about 10 seconds to 1 second) from the bulk deregistration path; review explicitly raised the compatibility impact of the Signal PDU filter-prefix change, and the author explained that the narrower prefix was required to avoid deregistering unrelated fields and to prevent filter-name collisions.

**Confidence:** High. The implementation and namespace change were merged after explicit discussion of both compatibility and ownership consequences.

## Re-register a mutated dynamic declaration set as a replacement, not an append

For scripting or other dynamic registration APIs, changing the declaration list after initial registration creates a replacement lifecycle problem. Existing registered fields or expert entries remain tied to the old arrays; simply appending new declarations can leave duplicate registrations, stale IDs, or backing arrays that cannot safely be reclaimed.

Merged master MR !15750 makes newly assigned Lua `Proto.fields` and `Proto.experts` visible after running code in the Evaluate window. If a `Proto` was already registered, the accepted implementation first deregisters the existing field/expert registration set and hands the old backing arrays to epan's deferred deregistered-data mechanism before constructing and registering the replacement arrays. John Thacker approved the merged change.

**Implementation rule:** when a dynamic declaration set can be mutated after registration, treat re-commit as replacement: deregister the previous registered objects through the supported API, preserve their backing storage for the required deferred lifetime, then register the new complete set. Do not assume that updating the script-visible collection updates the registration framework in place.

**Review implication:** exercise repeated mutation/commit cycles, including adding fields after initial registration. Check both registration identity (`hf_id`/expert IDs) and backing-storage lifetime; a result that merely makes the newest field visible can still contain a latent use-after-free or duplicate registration.

**Confidence:** Very high. Merged master WSLua lifecycle fix with explicit comments tying the replacement arrays to epan's deregistration lifetime rules, approved by John Thacker.

## Bind dynamic field registration to the declaration-set lifecycle, not the capture-file lifecycle

Dynamically registered fields defined by persistent configuration can outlive any one capture file from the user's point of view. Columns, coloring rules, and display filters may retain references to those fields across file close/open. Rebuilding the registration merely because the capture-file lifecycle restarted can therefore invalidate still-live references.

Merged master MR !8901, authored by John Thacker, moves HTTP/2 UAT-defined `hf_` replacement out of protocol file init/cleanup and into the UAT post-update/reset callbacks. The implementation deregisters the old dynamic fields when the declaration set changes, keeps static header registrations separate, and hands the old registration backing array to the deferred deregistration-data mechanism. John also reported an ASAN build with no leak. The later !19260 API/lifetime documentation remains the stronger authority for when deregistered field memory is actually reclaimable; !8901 supplies earlier concrete lifecycle evidence.

**Implementation rule:** tie registration replacement to the event that changes the field declarations. Do not treat opening or closing a capture as permission to tear down persistent dynamic field identities when UI/filter state can survive that transition.

**Confidence:** Very high. Merged master fix authored by John Thacker; later notebook evidence strengthens the same lifecycle contract.
