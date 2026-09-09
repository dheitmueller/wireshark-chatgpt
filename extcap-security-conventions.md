# Wireshark Extcap Security Conventions

This file records durable security conventions for Wireshark extcaps and shared remote-capture infrastructure. Current upstream source remains authoritative.

## Verify SSH server identity before sending authentication material

An extcap using SSH should authenticate the server before attempting password, private-key, or other client authentication. A successful TCP/SSH handshake is not sufficient: accepting an unverified host key exposes the credentials and capture session to an active man-in-the-middle.

Merged MR !26053, authored by John Thacker and merged after maintainer approval, adds common known-host verification to the shared `ssh-base` path used by Wireshark SSH extcaps. The verification occurs immediately after the SSH connection is established and before any client authentication material is sent.

The accepted behavior distinguishes the trust states rather than treating all non-success cases alike:

- a known matching key is accepted;
- a changed key is a hard verification failure and reports the new fingerprint;
- a mismatched key type is a hard failure;
- an unknown/missing host entry is rejected by default, with an explicit `--update-known-hosts` option allowing the user to add the fingerprint and continue;
- libssh verification errors are reported as failures;
- GSS-API key exchange is exempted where libssh reports that the server identity was already authenticated by the GSS exchange.

**Implementation rule:** server trust establishment precedes credential transmission. Unknown hosts require explicit user intent to trust/add; changed keys must not be silently replaced. Where the underlying SSH mechanism already cryptographically authenticates the server identity, avoid redundant host-key assumptions that the protocol/library explicitly says may not hold.

**Confidence:** Very high. Merged shared extcap security architecture authored by John Thacker and applied across the SSH-based extcaps.

## Scrub authentication material before releasing its storage

Freeing a password, private-key passphrase, or related SSH parameter does not erase the bytes from process memory. Treat explicit zeroization as part of the teardown contract for structures that own authentication material.

Merged MR !25709, authored by Gerald Combs, changes the common extcap SSH parameter destructor to zero its credential-related strings before `g_free()`. Merged follow-up !25716, authored by John Thacker and approved by Gerald, broadens the portable implementation to prefer primitives whose stores are specified not to be optimized away: C23 `memset_explicit`, Windows `SecureZeroMemory`, `explicit_bzero` on BSD/musl/older glibc, and `memset_s`, with ordinary `memset` only as a last-resort fallback.

**Implementation rule:** when a long-lived structure owns passwords, passphrases, or comparable authentication secrets, explicitly scrub the backing storage before freeing it. Feature-detect and prefer a platform primitive with non-elidable zeroization semantics; do not assume an ordinary `memset()` immediately before `free()` will survive optimization.

**Confidence:** Extremely high. Two merged master security changes, authored by Gerald Combs and John Thacker and converging on the same shared extcap teardown path.