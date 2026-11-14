# Mutation operators

**Status:** Specification. Not implemented. Each operator changes one thing in a valid authorization or record, and the scenario that uses it must then be refused or detected.

| ID  | Operator               | Changes                                                                | Scenario |
| --- | ---------------------- | ---------------------------------------------------------------------- | -------- |
| M01 | Amount change          | `parameters.amount` in the content                                     | T05, T20 |
| M02 | Target change          | `target` in the content                                                | T06, T21 |
| M03 | Policy substitution    | Live policy content, same name and version                             | T07, T26 |
| M04 | Signature bytes        | One character of the signature string, same decoded bytes or different | T12, T14 |
| M05 | Signer substitution    | Signature by a key outside the trust root                              | T13      |
| M06 | Nonce reuse            | Same nonce, different content                                          | T17      |
| M07 | Expiry shift           | Presentation time after `expiresAt`                                    | T02, T19 |
| M08 | Grant revocation       | Grant status set to revoked before presentation                        | T03, T18 |
| M09 | Record field change    | One field of a signed trust record                                     | T11      |
| M10 | Record removal         | Remove one completed execution's trust record                          | T22      |
| M11 | Dispatched hash change | `dispatchedContentHash` or its removal in the record                   | T20, T28 |
| M12 | Ledger truncation      | Drop the last k records from the ledger                                | T23      |

Rules for every operator:

- It changes exactly one thing, so a detection can be traced to it.
- It is applied to a baseline that passes unmodified, with the same fixtures and clock.
- The baseline and the mutated run are both recorded, so the pair can be checked.
- An operator with no detection is a finding, recorded as `failed`, not dropped.

Implementation status: none. M01 to M11 depend on the scenarios that are implemented. M12 depends on the ledger connection (decision D11 in `parmana-core`).
