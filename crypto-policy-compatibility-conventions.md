# Host Crypto-Policy Compatibility Notes

Wireshark merge request 9364 fixed capinfos aborting on systems where FIPS policy rejects RIPEMD160. Guy Harris asked whether FIPS mode could be disabled, while the contributor noted that some systems require that policy. Gerald Combs instead favored removing RIPEMD160 because the optional hash was already deprecated elsewhere, and the accepted change removed it from capinfos and updated the user documentation.

Durable convention: optional functionality does not bypass or disable host cryptographic policy merely to retain a deprecated algorithm. When a nonessential algorithm conflicts with required platform policy and has weak ongoing value, removing that feature and updating its documentation is preferable to weakening the environment's policy.

Confidence: extremely high.
