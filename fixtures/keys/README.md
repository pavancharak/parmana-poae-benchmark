# Test key material

These Ed25519 key pairs are for the PoAE benchmark only. They are not production keys and must never be used for signing real authorizations. The private keys are committed on purpose: they make the benchmark deterministic.

- `signer`: the key that signs authorizations in the scenarios.
- `untrusted`: a key that is not in the trust root, used for T01, T13 and CE-03 style cases.

Regenerating a pair changes every expected hash that depends on it. Regeneration is a reviewed change.
