# Reviewed Wireshark merge requests: !11213–!11262

- **Corpus commit reviewed:** `ddcaa22b51c68f594e425a23388c3a2086813054`
- **Review direction:** descending MR number, newest available previously-unreviewed first
- **Exact MR count:** 50
- **Exact selected set:** !11262, !11261, !11260, !11259, !11258, !11257, !11256, !11255, !11254, !11253, !11252, !11251, !11250, !11249, !11248, !11247, !11246, !11245, !11244, !11243, !11242, !11241, !11240, !11239, !11238, !11237, !11236, !11235, !11234, !11233, !11232, !11231, !11230, !11229, !11228, !11227, !11226, !11225, !11224, !11223, !11222, !11221, !11220, !11219, !11218, !11217, !11216, !11215, !11214, !11213
- **Tracking consulted before selection:** `reviewed-mrs.md`, `reviewed-mrs-automation/reviewed-mrs-automation.md`, and the available per-run files under `reviewed-mrs-automation/`, including the immediately preceding `reviewed-mrs-automation-11263-11312.md` and the historical/high-number per-run ledgers.
- **Historical tracking preserved:** the previously reviewed !17571–!17620 batch remains part of the already-reviewed set.
- **Prior frontier probe:** !11262 had only been inspected to establish the previous run's frontier and was not previously counted as reviewed.
- **Status mix:** 46 merged, 3 closed/unmerged (!11224, !11217, !11216), and 1 open in the corpus snapshot (!11255).

Selection was made from the individually reconstructed already-reviewed set, not by assuming numeric intervals were fully covered. Merged master changes are primary evidence. Stable-branch backports primarily corroborate accepted behavior. Closed, open, abandoned, and superseded proposals are down-weighted. Maintainer-authored or maintainer-approved changes—especially Guy Harris, John Thacker, Pascal Quantin, and other long-standing maintainers—receive correspondingly high weight.

## Exact review ledger

| MR | State | Review result |
|---|---|---|
| !11262 | merged | Deep/high weight. John Thacker extends the AMR codec to handle RFC 4867 octet-aligned RTP payloads containing multiple frames, rearranging the payload's table-of-contents/speech layout into the per-frame layout expected by OpenCORE-AMR while preserving explicit bounds checks and silence fallback for malformed/truncated input. Useful codec-boundary and malformed-input evidence; no new cross-cutting rule promoted. |
| !11261 | merged | SAMR `LookupRids` request parsing fix with a representative capture and before/after decode evidence. Straightforward DCERPC correctness work; no distinct general convention. |
| !11260 | merged | Lua test-suite refactor moves repeated helper logic into a shared library rather than allowing copied helpers to drift semantically. Good test-maintenance corroboration, but existing test architecture guidance is already stronger. |
| !11259 | merged | Guy Harris-authored Linux/libnl build fix restores the direct `<errno.h>` dependency where an errno value is used. Corroborates direct-header/self-contained dependency guidance. |
| !11258 | merged | macOS bundle script prepopulates its rpath search set. Packaging/runtime-linker maintenance; no new general rule. |
| !11257 | merged | Additional macOS app-bundle dependency/rpath fixes. Platform packaging maintenance. |
| !11256 | merged | Deep. Display-filter time arithmetic moves to checked `safe-math.h` operations. Discussion exposed GCC 4.8.5 lacking `_Generic`; João Valverde stated that Wireshark already required C11 including generics, so unsupported compilers should not constrain code below the declared language baseline. Corroborates existing compiler-baseline and checked-arithmetic guidance. |
| !11255 | open snapshot | Down-weighted/provisional. Proposed streaming-reassembly fix removes a temporarily added protocol layer when a child requests more data, motivated by first-pass/two-pass layer-number divergence. Includes targeted test plugin/captures, but unresolved/open status means it is not accepted architectural evidence. |
| !11254 | merged | Deep corroboration. SOME/IP-SD traffic-learned port registrations are now performed only on the first dissection pass using `!PINFO_FD_VISITED(pinfo)`, avoiding repeated persistent table mutation during redissection. Promoted with !11253 to `dissector-state-conventions.md`. |
| !11253 | merged | Deep/high weight. SOME/IP DTLS auto-detection dynamically registers the discovered DTLS port. Pascal Quantin explicitly required `dissector_add_uint()` to run on first pass only; the accepted diff adds `!PINFO_FD_VISITED(pinfo)`. Promoted to `dissector-state-conventions.md`. |
| !11252 | merged | IO Graph packet selection now converts correctly between absolute Time-of-Day display and relative graph-selection time. UI time-domain correctness fix; no broader rule needed. |
| !11251 | merged | release-4.0 backport of RTP Player resampler reinitialization when the input rate changes. Corroborates !11236. |
| !11250 | merged | ISO15765 display-filter/header-field naming cleanup removes stale FlexRay-specific names. Naming maintenance. |
| !11249 | merged | Deep. Long PLDM review produced several useful conventions: Jaap Keuter rejected mutable dissector module globals for transaction/session state and required conversation-scoped state; Alexis La Goutte pushed back on unnecessary fragmentation of several small dissector files and said test captures need not be committed with the source. The final MR was merged after extensive review. Conversation-state lesson strongly corroborates existing `dissector-state-conventions.md`; no duplicate rule added. |
| !11248 | merged | Bluetooth L2CAP adds the SMP fixed channel for BR/EDR. Review caught incorrect fixed-channel constants before merge. Protocol-specific correctness. |
| !11247 | merged | Martin Mathieson-authored RLC-LTE cleanup uses `_add_uint()` to avoid fetching the same TVB value twice. Corroborates existing typed-item/helper guidance. |
| !11246 | merged | Release-note APN6 URL update. Documentation-only. |
| !11245 | merged | macOS bundle search path adds `/usr/local/lib`. Packaging-only. |
| !11244 | merged | NCP dissector and generator migrate to C99 integer types. Type-modernization corroboration. |
| !11243 | merged | macOS bundle script removes a leftover background `&` and adds diagnostics. Packaging script correctness. |
| !11242 | merged | Deep/high-authority corroboration. Guy Harris removes unnecessary `<errno.h>` includes, but John Thacker's Rocky Linux 8 build report catches one source file that actually relied on the header through transitive includes. Guy explicitly identifies distro/header-version-dependent transitive inclusion as the cause. Strongly reinforces `public-header-validation-conventions.md`; no duplicate rule. |
| !11241 | merged | Martin Mathieson fixes additional full-width field masks that should not be masks. Corroborates typed-item/static-checker guidance. |
| !11240 | merged | Martin Mathieson fixes additional numeric label/filter mismatches. Corroborates field-registration consistency checks. |
| !11239 | merged | TECMP cleanup uses the common `base_unit` support for `FT_DOUBLE` and standardizes presentation. Field-presentation cleanup. |
| !11238 | merged | DNP3 Secure Authentication object support. The author supplied a pcapng with a broad collection of the newly supported objects; useful submission/testing evidence, but no new general rule. |
| !11237 | merged | release-4.0 backport of corrected Debian build instructions. Documentation backport. |
| !11236 | merged | John Thacker-authored RTP Player fix initializes/reinitializes the resampler according to the current input rate rather than assuming the first frame established permanent state. Accepted master correctness fix; no distinct notebook rule beyond existing state/lifetime guidance. |
| !11235 | merged | Deep corroboration. X.509 RFC 9310 ASN.1 addition: Pascal Quantin requested explicit provenance for the ASN.1 source, inclusion of regenerated `packet-x509ce.[ch]`, and a cleaned-up commit history. Strongly reinforces existing source-provenance/generated-code conventions. |
| !11234 | merged | release-4.0 CMake fix for Ninja + MSVC. Build portability backport. |
| !11233 | merged | DCERPC LSARPC `LsarOpenPolicy3` support with attached capture and request/response screenshots. Protocol enhancement with representative validation. |
| !11232 | merged | release-3.6 backport of Guy Harris's GlusterFS wire-status namespace fix. Corroborates !11228. |
| !11231 | merged | release-4.0 backport of Guy Harris's GlusterFS wire-status namespace fix. Corroborates !11228. |
| !11230 | merged | release-3.6 backport of Guy Harris's 9P2000.L wire errno decoding fix. Corroborates !11227. |
| !11229 | merged | release-4.0 backport of Guy Harris's 9P2000.L wire errno decoding fix. Corroborates !11227. |
| !11228 | merged | Deep/extremely high authority. Guy Harris replaces `g_strerror()` for GlusterFS status fields with a protocol-defined `GF_ERROR_CODE_*` value table. The MR explains that native errno values differ across OSes and even Linux architectures, while GlusterFS deliberately maps them into a stable wire namespace. Promoted to `platform-api-portability-conventions.md`. |
| !11227 | merged | Deep/extremely high authority. Guy Harris removes host `g_strerror()` decoding for 9P2000.L wire error numbers because errno numeric assignments are platform/architecture dependent. The accepted dissector uses a protocol-side Linux-value table instead. Promoted to `platform-api-portability-conventions.md`. |
| !11226 | merged | Master fix for Debian build instructions, requiring the packaging/debian link before `dpkg-buildpackage`. Documentation/build workflow maintenance. |
| !11225 | merged | Display-filter VM read-path refactor for modularity. Internal cleanup without a distinct review-derived convention. |
| !11224 | closed | Down-weighted. Proposed disabling of a runnerless Intel macOS CI job was closed rather than merged; useful historical CI context only. |
| !11223 | merged | Qt Help menu actions are connected explicitly rather than relying on automatic slot naming. Qt maintenance. |
| !11222 | merged | Merged UI change replaces a lossy/encoding-ambiguous byte-array “Printable Text” copy action with valid escaped output. Later user discussion documented substantial SIP workflows that depended on the removed behavior and requested restoration. Because accepted implementation and later compatibility feedback pull in different directions, no durable UI policy is inferred from this MR alone. |
| !11221 | merged | Qt Wireless/Tools actions move to explicit signal/slot connections and avoid names that resemble auto-connect conventions. Framework-specific cleanup. |
| !11220 | merged | SVCCTL shadow-variable warning cleanup. Mechanical correctness/style fix. |
| !11219 | merged | Automatic master registry/update job; description records a failed services sub-update. Routine generated-data maintenance. |
| !11218 | merged | `make-pci-ids.py` generated output now includes its direct `stddef.h` dependency. Corroborates self-contained generated output/header dependency guidance. |
| !11217 | closed | Down-weighted/superseded. Initial PLDM dissector submission was closed; the substantive work continued and ultimately merged as !11249. |
| !11216 | closed | Down-weighted. Automatic master update was closed and replaced by the successful follow-up update. No architectural evidence. |
| !11215 | merged | Automatic release-4.0 registry/data update. Routine generated-data maintenance. |
| !11214 | merged | Automatic release-3.6 registry/data update. Routine generated-data maintenance. |
| !11213 | merged | John Thacker-authored CI change moves the fast license check into the early Commit Check job so dependent builds abort before CMake/compilation when licensing fails; Gerald Combs approved. Strong corroboration of the existing fail-fast CI convention already represented by later evidence. |

## Durable notebook updates

- **!11227 + !11228, corroborated by stable !11229–!11232 — decode wire status values in the protocol namespace, not the host's errno namespace.** Host `strerror()` semantics are not a portable decoder for on-wire numbers. Added to `platform-api-portability-conventions.md` in commit `141853722cdce622f3f8590fe0b910cdce3f36e5`.
- **!11253 + !11254 — packet-learned persistent dissector-table registrations belong to the first dissection pass.** Pascal Quantin explicitly identified the repeated `dissector_add_uint()` redissection side effect, and both accepted SOME/IP changes use `!PINFO_FD_VISITED(pinfo)` around traffic-learned registrations. Added to `dissector-state-conventions.md` in commit `c6121477c6ccc76f1ecfdc33a6b0aea5bb8a8ec9`.

## Strong corroboration retained without duplicate notebook rules

- **!11249:** per-session dissector state cannot live in mutable module globals; map it to conversation/protocol state. Existing state guidance is already stronger and broader.
- **!11242 + !11259 + !11218:** direct dependency headers must be included explicitly; transitive inclusion differs across systems and generated output should be self-contained. This reinforces `public-header-validation-conventions.md`.
- **!11235:** checked-in generated ASN.1 output must travel with authoritative/provenanced source input and be reproducible. This reinforces `source-provenance-conventions.md` and `generated-code-conventions.md`.
- **!11256:** code may use features guaranteed by Wireshark's declared C language baseline; an obsolete compiler that lacks those features does not silently redefine the supported baseline.
- **!11213:** move fast deterministic policy checks ahead of expensive dependent builds so CI fails early. Existing `ci-tooling-conventions.md` already records this practice with later evidence.

## Down-weighted or superseded evidence

- **!11255** remained open with unresolved discussion in the corpus snapshot; its reassembly/layer proposal is provisional rather than accepted behavior.
- **!11224** was closed without merge.
- **!11217** was superseded by the extensively reviewed and merged !11249.
- **!11216** was a closed automatic-update attempt superseded by the successful update series.
- **!11222** was merged, but significant post-merge user feedback documented compatibility/workflow costs, so it is not used as a durable UX-removal policy.

## Frontier check

MR **!11212** (`Qt: Manually connect our "Go To Packet" buttons`) exists in the same corpus commit and is merged. It was inspected only to verify that the corpus continues below this batch; **it is not counted among these 50 reviewed MRs** and remains eligible for the next descending run.
