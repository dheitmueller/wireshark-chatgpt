# Review findings: Wireshark MRs !9663–!9712

Corpus commit: `ddcaa22b51c68f594e425a23388c3a2086813054`.

This run reviewed the exact 50-MR set in `reviewed-mrs-automation-9663-9712.md`. Merged work was weighted above closed experiments.

## Durable findings

**!9705 and !9711 — UTF-8 helpers must preserve the enclosing format's semantics.** !9705 fixes pcapng string-option handling because GLib's `g_utf8_make_valid()` treats embedded NULs differently from pcapng. The accepted change moves Wireshark's own UTF-8 repair behavior into `wsutil/unicode-utils`; John Thacker validated it against a real Apple-produced pcapng sample. In !9711 Gerald Combs asked whether direct GLib use should be banned by `checkAPIs.pl`; João Valverde preferred, at most, a gentle hint rather than a blanket ban. Use shared helpers when their semantics match the file/protocol contract, but do not outlaw a general API when it remains valid in other contexts.

**!9701 — redissection must not advance live first-pass state.** USBLL had been restoring old per-packet state into the same global pointer used for current first-pass capture progress. The merged fix restores revisited-packet state into a local variable and updates the global progression pointer only on first pass. Separate "state for this packet now" from "state representing current sequential capture progress."

**!9690 — distinguish unsupported from not-yet-known state.** John Thacker introduces `WTAP_ENCAP_NONE` for the legitimate streaming state in which encapsulation has not yet been established, while `WTAP_ENCAP_UNKNOWN` remains for unsupported/bogus encapsulation. A writer can defer a decision for the former but not the latter. Do not overload a single sentinel when downstream behavior differs.

**!9685 — aggregate capability does not prove every contained value is supported.** A format may support per-packet encapsulation while still being unable to encode a particular concrete encapsulation. The accepted merge path validates each input encapsulation against the output file type before proceeding.

**!9669 — end of packets is not necessarily end of container metadata.** tshark, editcap, and mergecap now process Interface Description Blocks discovered after the final packet record. A streaming pipeline should drain meaningful trailing metadata before finalizing output.

**!9688 — local/vendor file-format semantics belong behind a registration boundary.** The pcapng core keeps common framing/dispatch while Apple's Darwin Process Information block moves to its own registered handler. Generic container parsers should own framing; extension semantics should register through an explicit child mechanism.

**!9697 — dependency baselines should follow the project's supported-platform contract.** The Qt 5.11 minimum-version discussion weighed actual distribution baselines and other project dependencies, rather than treating every older deployed system as necessarily supported. The MR also stopped short of Qt 5.12 because there was no technical need and Debian Buster still used 5.11.3.

**!9692 — review captures and committed test assets are separate decisions.** Stig Bjørlykke requested a representative CoAP capture. Alexis La Goutte clarified that it could be attached to the MR rather than committed under `test/captures` when no maintained automated test consumed it. Late review also found issues after merge; follow-up !9731 corrected them.

## Additional corroboration

!9710 applies XML 1.0 character rules rather than generic C whitespace assumptions. !9709 is the accepted MSYS2 build-symbol approach after closed !9708. !9707 and !9706 remove wrapper/header indirection after its original purpose disappeared. !9704 makes UAT terminology and selection explicitly about dissectors rather than protocols. !9703 finds and fixes real typed-item field-width mismatches. !9702 includes a representative MySQL clone capture and fixes non-portable syntax exposed by Windows CI. !9694 shows that one display-filter name cannot safely represent incompatible field types. !9696/!9695/!9693 fix RTP sorting to use the actual signed loss value. !9687/!9686/!9680 are repeated stable fixes for an ISO15765/ISO10681 array-bounds defect. !9679/!9678 keep setup-script interfaces aligned across CI systems. !9677 places a transfer-ending workaround at the layer that knows the protocol semantics. Guy Harris's !9676/!9675/!9674/!9673/!9672/!9670 tshark sequence shows both the benefit of factoring duplicated filter-warning logic into one helper and the need to verify the correct variable at every call site. !9668/!9667/!9666 show that nested dissections may legitimately have empty packet endpoint addresses. !9664/!9663 show that enum-to-combobox positional coupling requires synchronized ordering.

## Outcome summary

48 merged; two closed/unmerged (!9712 and !9708). No open MRs in the batch. !9662 remains below the reviewed frontier.
