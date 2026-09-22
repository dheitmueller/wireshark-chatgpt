# Wireshark Displayed-Frame Context Conventions

This file records durable conventions for analysis values whose semantics depend on the displayed/selected frame set rather than raw capture order. Current upstream source remains authoritative.

## View-relative metrics must carry view-relative predecessor state

Fields whose names and documented behavior are based on displayed packets must be computed against the current displayed/filtered frame sequence, not against the immediately preceding frame in the capture. Any API that walks a sparse or filtered set must therefore maintain the same predecessor/reference state that the full UI path would maintain.

Merged master MR !13116 fixes `sharkd`'s `frames` method after `frame.time_delta_displayed` began using packet 799 as the predecessor for a filtered result containing only packets 1 and 800. The accepted code restores `prev_dis_num`, `current_ref_frame`, and `ref_frame` state while processing the requested result set. Its regression test verifies that packet 800 reports the delta from displayed packet 1 rather than the raw-capture delta from packet 799. Guy Harris approved the master fix. Merged release backport !13139 carries the same behavior and test to an older branch, reinforcing that this was treated as stable semantic correctness rather than a cosmetic presentation change.

**Implementation rule:** distinguish capture-order context from displayed-order context explicitly. If a computed field is defined in terms of visible/displayed packets, every frontend or API that independently iterates packets must update the displayed predecessor/reference state according to its own filter/result set.

**Testing rule:** use a deliberately sparse display filter so the previous displayed frame and previous captured frame are far apart. Assert both ordinary capture delta and displayed delta so a regression cannot pass by accidentally making the two sequences identical.

**Compatibility rule:** headless/front-end APIs such as sharkd must preserve the same analysis semantics as the desktop UI for fields exposed through both interfaces; filtering in the transport/API layer must not silently change what a field means.

**Confidence:** Very high. Merged master correctness fix with a targeted regression test, Guy Harris approval, and merged release backport.

## Timestamp-relative state advances only on records with timestamps

Capture files can contain a mixture of timestamped and untimestamped records. An untimestamped record must not be treated as if it had timestamp zero, nor may it replace the previous/reference timestamp used for subsequent delta calculations.

Merged release-4.2 MR !12996, authored by John Thacker, fixes frame delta/reference handling so records without timestamps are excluded from timestamp-predecessor state. Frame timestamp getters return no timestamp for such records, and the next timestamped frame continues relative to the last valid timestamped predecessor. Other non-time state, such as cumulative-byte or display bookkeeping, can still advance independently where its semantics require it.

**Implementation rule:** represent timestamp availability explicitly. Update time-reference and previous-time state only when the current record has a valid timestamp; keep non-time ordering/state separate rather than letting record ordinal position imply time availability.

**Testing rule:** exercise timestamped, untimestamped, then timestamped records in one capture and verify that the final delta is calculated from the preceding valid timestamp, not from the intervening untimestamped record.

**Confidence:** Very high. Merged correctness fix authored by John Thacker for a concrete mixed-record capture behavior.
