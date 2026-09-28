# Wireshark Cleanup-Ownership Conventions

This file records durable conventions for teardown of composite objects and resources with distinct ownership contracts. Current upstream API documentation and implementations remain authoritative.

## Let the owning cleanup routine free the members it owns

A composite cleanup function defines an ownership boundary. Callers must not manually free a member that the composite cleanup routine will subsequently release, even if that member also has a public standalone destructor. Doing both turns an apparently careful cleanup path into a double free.

Merged master MR !11546, authored and merged by Guy Harris, fixes Wiretap dump-parameter cleanup where callers explicitly freed `params.shb_hdrs` and then called `wtap_dump_params_cleanup(&params)`. The higher-level cleanup already owns and frees the section-header block array, so the explicit prior free caused a double free. The same change keeps IDB-info cleanup separate with `wtap_free_idb_info()` and introduces a text2pcap helper that performs the required teardown sequence consistently on failure paths.

**Ownership rule:** map each resource to exactly one owning destructor at a given lifecycle boundary. If a composite object's cleanup owns a member, invoke the composite cleanup and do not pre-free that member independently.

**Composition rule:** when a logical operation owns several resources that require different destructors, centralize the exact teardown sequence in one helper or owning abstraction. This is especially important when there are multiple early returns or error paths; duplicating partial cleanup sequences makes double frees and leaks more likely.

**Review rule:** when fixing a leak or adding cleanup, inspect the implementation/contract of existing higher-level cleanup functions before adding member-level destructors. “Free everything visible” is not safe when ownership is nested.

**Confidence:** Extremely high. Merged master memory-safety fix authored and merged by Guy Harris, with the duplicate ownership and correct cleanup decomposition stated directly in the change.


## Register an allocated private object with its owner before later initialization can fail

When an owning object is responsible for final cleanup, attach a newly allocated private object to that owner immediately after allocation and establish the owner's cleanup callback before performing later operations that can fail. Delaying the ownership link until the end of initialization creates an error-path window in which normal teardown cannot see the allocation.

Merged master MR !5911, authored by Guy Harris, fixes `libpcap_open()` by setting `wth->priv`, the read/seek/close callbacks, and the snapshot metadata before the later fallible initialization steps. It also zero-initializes the private `libpcap_t`. The explicit purpose is to ensure Wiretap's normal error cleanup can free the private object even when opening fails partway through.

**Ownership rule:** once allocation succeeds, establish the owning object's pointer and destructor/close path before the next operation that can fail. Error cleanup should be able to use the same ownership graph as success cleanup rather than depending on ad-hoc frees for partially initialized state.

**Initialization rule:** when the cleanup routine may inspect fields of a partially initialized private object, prefer zero-initialization so untouched members begin in a safe neutral state.

**Confidence:** Extremely high. Merged master resource-lifetime fix authored by Guy Harris and motivated by a concrete Coverity leak finding.
