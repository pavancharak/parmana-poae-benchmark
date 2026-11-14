# Deterministic fixtures (plan)

**Status:** Keys and the fixed clock implemented. Policies and grants are still planned.

The runner in `runner/` currently generates a new key pair on every run (`generateKeyPairSync`). That makes a run reproducible in behaviour, but not byte for byte. Phase 2 requires fixed fixtures:

- fixed Ed25519 key pairs for the signer and an untrusted signer, in `keys/`, test material only. **Done.** The runner reads them through `POAE_KEYS_DIR`, and the workflow sets it;
- fixed policy documents and their content hashes;
- fixed grant records (active, revoked, expired), with fixed validity times;
- fixed authorization payloads for each scenario in `scenarios/definitions.md`;
- a fixed clock for expiry checks. **Done:** the runner sets the Date clock to 2026-11-14T12:00:00Z for the run (vitest fake Date), so no product code changes.

Each fixture file records the commit of `parmana-core` it was generated against. Regenerating a fixture is a reviewed change.
