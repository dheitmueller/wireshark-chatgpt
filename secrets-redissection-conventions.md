# Secrets and Redissection Conventions

This note captures review-backed guidance for secret material that becomes available as part of capture dissection rather than before capture loading.

## Prefer the shared secrets pipeline over bespoke pre-dissection machinery

When a capture format or protocol carries TLS key-log material in-band, prefer converting that material into Wireshark's existing secrets infrastructure and Decryption Secrets Block workflow instead of adding a parallel TLS-specific or capture-prescan path merely to make the secret available before the first TLS pass.

Merged MR !25391 is the strongest exemplar. The F5 Ethernet trailer postdissector feeds discovered TLS key-log lines into the existing secrets machinery. If handshake secrets occur after the packets that need them, the user can inject the collected secrets and redissect; the capture can also be saved as pcapng with a DSB containing those secrets. The TLS dissector itself does not need F5-specific knowledge.

Merged MR !25393 extends the same model for Encrypted Client Hello: it records the outer Client Random and exports matching ECH secret/config material so the resulting injected secret set is complete enough to be useful on redissection and when saved as a DSB.

Closed MR !25386 is useful negative contrast, but should receive less weight because it was not merged. It proposed detecting F5 captures at file open and doing a dedicated prescan to collect key-log data before ordinary dissection. The discussion did produce a smaller reusable lesson—John Thacker recommended using the existing `ws_memmem` helper rather than a local search implementation—but the later merged !25391 demonstrates the preferred architecture for this case: reuse the shared secret collection/injection path and allow redissection to resolve ordering.

### Practical rule

Before adding a prescan, frontend hook, or consumer-specific decryption path for late-discovered secrets, check whether the existing secrets/DSB infrastructure can represent the data and whether redissection can make the newly available state effective. A separate pre-dissection path is justified only when the normal shared-state-and-redissection lifecycle cannot satisfy the required semantics.

This also keeps protocol ownership clean: the producer of the secret material identifies and normalizes it, while TLS consumes secrets through its normal generic interface.

## Export only capture-relevant secrets, and coordinate new persistent secret formats externally

A Decryption Secrets Block can outlive the process and capture environment that produced it. When a protocol has a larger configured secret database than the capture actually needs, do not automatically serialize the whole database merely because it is available in memory. Track which secrets were actually selected/used during dissection and export only that relevant subset when practical.

Merged MR !20256 adds ESP Security Association persistence through pcapng DSBs. During review, Pascal Quantin changed the implementation so only ESP SAs actually used by the capture are exported, rather than all configured SAs. John Thacker also asked whether the new secrets type was being added to the pcapng specification; the author subsequently opened the corresponding external specification proposal before the Wireshark MR ultimately merged.

**Implementation rule:** treat persistent secret export as a disclosure boundary. Prefer the minimum secret set required to reproduce decryption of the saved capture rather than serializing unrelated configured credentials.

**Format rule:** if a new DSB secret type changes an interoperable capture-file format, coordinate the identifier/representation with the format's external specification or registry rather than treating a Wireshark-local constant as sufficient definition.

**Confidence:** Very high. Merged master feature by Pascal Quantin after extended review, with both secret minimization and external pcapng specification work explicitly reflected in the MR discussion.
