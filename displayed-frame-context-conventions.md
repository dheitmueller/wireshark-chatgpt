# Wireshark Displayed-Frame Context Conventions

This file records durable conventions for analysis values whose semantics depend on the displayed/selected frame set rather than raw capture order. Current upstream source remains authoritative.

## View-relative metrics must carry view-relative predecessor state

Fields whose names and documented behavior are based on displayed packets must be computed against the current displayed/filtered frame sequence, not against the immediately preceding frame in the capture. Any API that walks a sparse or filtered set must therefore maintain the same predecessor/reference state that the full UI path would maintain.

Merged master MR !13116 fixes `sharkd`'s `frames` method after `frame.time_delta_displayed` began using packet 799 as the predecessor for a filtered result containing only packets 1 and 800. The accepted code restores `prev_dis_num`, `current_ref_frame`, and `ref_frame` state while processing the requested result set. Its regression test verifies that packet 800 reports the delta from displayed packet 1 rather than the raw-capture delta from packet 799. Guy Harris approved the master fix. Merged release backport !13139 carries the same behavior and test to an older branch, reinforcing that this was treated as stable semantic correctness rather than a cosmetic presentation change.

**Implementation rule:** distinguish capture-order context from displayed-order context explicitly. If a computed field is defined in terms of visible/displayed packets, every frontend or API that independently iterates packets must update the displayed predecessor/reference state according to its own filter/result set.

**Testing rule:** use a deliberately sparse display filter so the previous displayed frame and previous captured frame are far apart. Assert both ordinary capture delta and displayed delta so a regression cannot pass by accidentally making the two sequences identical.

**Compatibility rule:** headless/front-end APIs such as sharkd must preserve the same analysis semantics as the desktop UI for fields exposed through both interfaces; filtering in the transport/API layer must not silently change what a field means.

**Confidence:** Very high. Merged master correctness fix with a targeted regression test, Guy Harris approval, and merged release backport.
