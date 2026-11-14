# Grant fixtures

`grants.json` holds three grants for the caller `agent-a` and the capability `TransferFunds`, with fixed times:

| Grant           | State at 2026-11-14T12:00Z               |
| --------------- | ---------------------------------------- |
| `grant-active`  | Active, in force                         |
| `grant-revoked` | Revoked on 2026-11-13                    |
| `grant-expired` | Active status, but expired on 2026-11-10 |

The shape follows the `AuthorityGrant` type in `packages/shared/src/domain/authority-grant.ts` in `parmana-core`. The runner currently builds its own grant registry inline (`grant-1`). Moving it to these fixtures is a planned change, not yet made.
