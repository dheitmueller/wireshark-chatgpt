# Wireshark Cleanup-Ownership Conventions

This file records durable conventions for teardown of composite objects and resources with distinct ownership contracts. Current upstream API documentation and implementations remain authoritative.

## Let the owning cleanup routine free the members it owns

A composite cleanup function defines an ownership boundary. Callers must not manually free a member that the composite cleanup routine will subsequently release, even if that member also has a public standalone destructor. Doing both turns an apparently careful cleanup path into a double free.

Merged master MR !11546, authored and merged by Guy Harris, fixes Wiretap dump-parameter cleanup where callers explicitly freed `params.shb_hdrs` and then called `wtap_dump_params_cleanup(&params)`. The higher-level cleanup already owns and frees the section-header block array, so the explicit prior free caused a double free. The same change keeps IDB-info cleanup separate with `wtap_free_idb_info()` and introduces a text2pcap helper that performs the required teardown sequence consistently on failure paths.

**Ownership rule:** map each resource to exactly one owning destructor at a given lifecycle boundary. If a composite object's cleanup owns a member, invoke the composite cleanup and do not pre-free that member independently.

**Composition rule:** when a logical operation owns several resources that require different destructors, centralize the exact teardown sequence in one helper or owning abstraction. This is especially important when there are multiple early returns or error paths; duplicating partial cleanup sequences makes double frees and leaks more likely.

**Review rule:** when fixing a leak or adding cleanup, inspect the implementation/contract of existing higher-level cleanup functions before adding member-level destructors. “Free everything visible” is not safe when ownership is nested.

**Confidence:** Extremely high. Merged master memory-safety fix authored and merged by Guy Harris, with the duplicate ownership and correct cleanup decomposition stated directly in the change.
