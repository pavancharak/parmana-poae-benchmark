# Grant fixtures

`grants.json` holds three grants for the caller `agent-a` and the capability `TransferFunds`, with fixed times:

| Grant           | State at 2026-11-14T12:00Z               |
| --------------- | ---------------------------------------- |
| `grant-active`  | Active, in force                         |
| `grant-revoked` | Revoked on 2026-11-13                    |
| `grant-expired` | Active status, but expired on 2026-11-10 |

The shape follows the `AuthorityGrant` type in `packages/shared/src/domain/authority-grant.ts` in `parmana-core`. The runner currently builds its own grant registry inline (`grant-1`). Moving it to these fixtures is a planned change, not yet made.

## Capability name: not confirmed

The grants use the capability `TransferFunds`. This is a **placeholder**, not a verified name. Checked in `parmana-core`:

- The `customer-refund` policy files have no capability field.
- `CANONICAL_CAPABILITY_POLICY_BINDINGS` is empty in this version, as its own comment states.
- The other capability-to-policy mapping comes from external connector registrations (ADR-0013), which are not in the repository.

So the repository does not say which capability governs `customer-refund`. Before the grant fixtures are used for a refund scenario, the capability name must be decided and recorded here.
