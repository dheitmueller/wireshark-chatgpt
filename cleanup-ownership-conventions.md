# Wireshark Cleanup-Ownership Conventions

This file records durable conventions for teardown of composite objects and resources with distinct ownership contracts. Current upstream API documentation and implementations remain authoritative.

## Let the owning cleanup routine free the members it owns

A composite cleanup function defines an ownership boundary. Callers must not manually free a member that the composite cleanup routine will subsequently release, even if that member also has a public standalone destructor. Doing both turns an apparently careful cleanup path into a double free.

Merged master MR !11546, authored and merged by Guy Harris, fixes Wiretap dump-parameter cleanup where callers explicitly freed `params.shb_hdrs` and then called `wtap_dump_params_cleanup(&params)`. The higher-level cleanup already owns and frees the section-header block array, so the explicit prior free caused a double free. The same change keeps IDB-info cleanup separate with `wtap_free_idb_info()` and introduces a text2pcap helper that performs the required teardown sequence consistently on failure paths.

**Ownership rule:** map each resource to exactly one owning destructor at a given lifecycle boundary. If a composite object's cleanup owns a member, invoke the composite cleanup and do not pre-free that member independently.

**Composition rule:** when a logical operation owns several resources that require different destructors, centralize the exact teardown sequence in one helper or owning abstraction. This is especially important when there are multiple early returns or error paths; duplicating partial cleanup sequences makes double frees and leaks more likely.

**Review rule:** when fixing a leak or adding cleanup, inspect the implementation/contract of existing higher-level cleanup functions before adding member-level destructors. “Free everything visible” is not safe when ownership is nested.

**Confidence:** Extremely high. Merged master memory-safety fix authored and merged by Guy Harris, with the duplicate ownership and correct cleanup decomposition stated directly in the change.


## A function's return ownership must be uniform across all successful paths

Callers need one ownership rule for one API. Returning a static string for some enum values and heap-allocated strings for others is unsafe when callers follow a single documented/observed cleanup convention.

Merged master MR !494, authored and merged by Guy Harris, fixes `topic_action_url()`: callers assume the returned string is allocated and free it, so every switch arm is changed to return duplicated/allocated storage even when the source URL is a compile-time constant. Stable-branch MRs !495 and !496 carry the same fix.

**Ownership rule:** if callers own and free a returned object, all successful return paths must provide the same ownership. Do not make ownership depend on which enum arm, protocol subtype, cache hit, or special case produced the value.

**Review rule:** when a function can return either borrowed/static or owned storage, encode that distinction in separate APIs/types if both behaviors are really required; otherwise normalize the implementation to one contract.

**Confidence:** Extremely high. Merged Guy Harris master fix plus two stable backports.
