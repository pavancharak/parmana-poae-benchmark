# Deterministic fixtures (plan)

**Status:** Plan. Not implemented.

The runner in `runner/` currently generates a new key pair on every run (`generateKeyPairSync`). That makes a run reproducible in behaviour, but not byte for byte. Phase 2 requires fixed fixtures:

- fixed Ed25519 key pairs for the signer, the trust root, and a second, untrusted signer, stored here as test material only (never production keys);
- fixed policy documents and their content hashes;
- fixed grant records (active, revoked, expired), with fixed validity times;
- fixed authorization payloads for each scenario in `scenarios/definitions.md`;
- a fixed clock for expiry checks, so the same run gives the same result on any day.

Each fixture file records the commit of `parmana-core` it was generated against. Regenerating a fixture is a reviewed change.
