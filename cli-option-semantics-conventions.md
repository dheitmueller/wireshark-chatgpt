# Wireshark CLI Option Semantics Conventions

This file records durable conventions for command-line option semantics and the work those options should cause. Current upstream frontend behavior remains authoritative.

## Make expensive processing follow the option's observable semantics

A command-line flag should cause only the work required to produce its documented observable effect. Merely seeing an option associated with a downstream feature is not, by itself, a reason to enable an expensive processing stage when the invocation has no output or filter that can consume that feature.

Merged master MR !13435, authored and merged by John Thacker, fixes TShark so `--color` alone does not force packet dissection. Dissection remains enabled when output, packet details, coloring-rule reporting, or a filter actually requires it. Guy Harris's review sharpened the user-facing side of the same contract: if `--color` is supplied in an invocation with nothing to color, the appropriate behavior is to tell the user that the option has no effect rather than silently doing unnecessary dissection.

**Execution rule:** derive expensive frontend work from the effective output/filter requirements after option parsing, not from the mere syntactic presence of a feature-related flag.

**Diagnostic rule:** when a valid option is semantically inert in the selected mode, prefer a clear warning or error where that helps the user identify a mistaken invocation. Do not manufacture work solely to make an otherwise ineffective option appear meaningful.

**Review rule:** for options that influence formatting, coloring, name resolution, decryption, reassembly, taps, or dissection depth, trace the option to the actual consumer and ask whether every invocation that carries the flag needs that processing stage.

**Testing rule:** cover the option both with and without each consumer that can make it effective. For performance-sensitive frontends, verify the no-consumer form does not unexpectedly enter packet dissection or another expensive pipeline stage.

**Confidence:** Extremely high. Merged master behavior fix authored and merged by John Thacker, with direct user-facing semantic guidance from Guy Harris.

## Share option definitions according to semantic capability, not incidental callers

When several Wireshark frontends intentionally expose the same command-line spelling and meaning, centralize the option definition at the layer representing that capability. Do not group an option under an unrelated subsystem merely because one current executable happens to exercise both.

Merged master MR !12569, authored and merged by John Thacker, fixes the mismatch where TShark's documentation advertised `--read-file`, `--read-filter`, and `--display-filter` but the parser did not actually accept all of those long forms. The change also factors common option definitions so Wireshark-family frontends can stay consistent. During review, Guy Harris specifically objected to grouping `-r`/`--read-file` as a dissection option: reading a capture file is an input-source capability, not a consequence of dissecting it. John Thacker agreed and separated the read-file definition while keeping filter options with the common dissection-capable frontend definitions. Gerald Combs also requested test coverage, and the accepted MR added a focused long-option test.

**Architecture rule:** place shared CLI option constants/macros in the common layer that owns the option's meaning. Use semantic capability boundaries—capture-file input, packet dissection/filtering, output, capture, and so on—rather than executable names or coincidental implementation coupling.

**Consistency rule:** implementation, `--help` output, and man pages form one interface contract. If documentation claims a long or short spelling exists, parser tables must accept it; if multiple frontends deliberately share the option, keep the spelling and semantics synchronized through shared definitions where practical.

**Testing rule:** when adding or refactoring shared option spellings, exercise the public spelling through the affected executable rather than testing only the underlying option constant. Include long-form aliases when the documentation promises them.

**Confidence:** Extremely high. Merged master change authored and merged by John Thacker, with direct architectural correction from Guy Harris and explicit test-coverage review from Gerald Combs.

## Reject unknown names inside structured command-line options

When an option accepts a secondary name or mini-language (for example `-b name:value`), recognizing the outer option is not enough: an unrecognized inner name is invalid input and should fail explicitly rather than being silently accepted and ignored.

Merged master MR !12274 changes the shared ring-buffer parser so an unrecognized `-b` sub-option returns failure. This makes TShark and dumpcap diagnose the bad invocation and deliberately aligns `-b` with the existing behavior of `-a`.

**Parsing rule:** parsers for structured option payloads should return success only when the supplied sub-option was actually recognized and accepted. Keep the failure result visible to the frontend so it can produce the normal usage/error path.

**Consistency rule:** equivalent option families exposed by multiple Wireshark frontends should reject malformed or unknown sub-options consistently; do not let one frontend silently ignore a spelling that another reports as invalid.

**Testing rule:** include at least one unknown sub-option/name in CLI tests, in addition to valid names and malformed values, so future additions do not accidentally turn the parser into an accept-and-ignore path.

**Confidence:** High. Merged master behavior change, approved and merged by Anders Broman.

## Preserve per-selector semantics when repeatable CLI filters are combined

A command line can accept multiple instances of options that select the same family of output fields but carry **different behavior flags**. Storing one global flag for all selected keys silently loses their distinct semantics, particularly when the options can be mixed in one invocation.

Merged John Thacker MR !9801 changes TShark `-j` and `-J` filtering from a global `pf_flags` plus a string array to a `wmem_map_t` of protocol abbreviations to **per-key** flags. `epan/print.c` consults that map for PDML, JSON, and Elasticsearch-style output. When a matched selector requests descendants, the printer temporarily relaxes the filter during subtree traversal and **restores** it afterward. A null map means no filtering. The change updates the interfaces used by TShark and related frontends, rather than merely accepting repeated command-line spellings.

**Review/testing rule:** test repeated `-j`, repeated `-J`, mixed option orders, different keys with different include-children policy, unmatched keys, and subsequent sibling nodes to catch leaked traversal state. Keep the option parser, shared output API, and recursive serializers consistent.

Separate open MR !9799 proposes `-X` extension-option validation. Gilbert Ramirez correctly identified duplicated file-type listing logic across Wireshark and TShark and recommended factoring common code into `wsutil`; that architectural feedback is useful, but the **open proposal must not be treated as an accepted CLI behavior change**.

**Confidence:** Very high for merged !9801 authored/merged by John Thacker; provisional for the implementation of open !9799, with maintainer design feedback recorded independently.

## Separate a user's expression from option parsing with the standard terminator

Merged master !9255, authored and merged by João Valverde, fixes `dftest` when a display filter begins with a minus sign, such as `-2 == tcp.port`. The test runner must execute `dftest -- <expression>` so the tool does not mistake a valid expression for a command-line option. The MR also replaces the earlier GLib option parser with a conventional `getopt` implementation; its own debug flags were introduced by !9250.

**CLI/testing rule:** when a harness passes arbitrary user-language expressions or filenames to an option-parsing executable, use `--` before the payload and verify its position against the tool's accepted grammar. Test negative-leading literals and values that resemble switches, in addition to ordinary arguments. A front-end's option parser must not silently narrow the valid syntax accepted by its underlying language.

**Confidence:** High: merged tool change and direct regression-test adjustment.
