# Conventions from MRs 8712-8761

Custom-column filter expressions should be generated from the underlying typed field occurrences, not reconstructed from rendered column text. Merged MRs 8725 and 8760 establish this because rendered text can lose escaping, occurrence boundaries, and field identity.

Protocol-tree string values and display labels are separate. The rejected 8719 direction changed the stored IPP value; merged 8731 reverted it and merged 8724 stores the real decoded string while applying descriptive text only to presentation.

Conversation API migrations must preserve the old endpoint/key semantics. Closed 8740 used a generic constructor as a compile fix; merged 8741 uses the endpoint-specific replacement after review.

When generic dispatch cannot pass required parent framing facts directly, packet/layer-scoped protocol data is appropriate. Merged master 8756 passes RTP payload and padding context to EVS; release backport 8767 corroborates the same design.

Custom CMake extension variables and the shipped example should expose the semantic source categories the build actually supports. Merged 8748 adds custom header/support hooks and merged 8753 exposes the already-supported custom TShark tap hook in the example.
