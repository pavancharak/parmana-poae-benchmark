## 12. Review record: PoAE formal security specification v0.1 (proposed)

Checked on 14 November 2026 against commit `4a29854` and the working tree of `claude/lucid-sagan-1gs4xj`. The proposal is research direction and has not been adopted. Nothing below changes the score policy, the phase exit criteria or the acceptance principles.

### 12.1 Claims checked

| Claim in proposal                                                                                        | Result                                                                                                                                                                                             | Evidence                                   |
| -------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| Packages `@parmanasystems/governance`, `execution`, `crypto`, `bundle`, `verifier`, `audit-db`, `server` | Incorrect names, as in earlier proposals. `crypto` exists. Others need the mapping in the standard, Section 3.                                                                                     | `packages/*/package.json`                  |
| Authorization binds policy bundle hash                                                                   | Not found. No source file matches `policyBundleHash`, `bundleHash` or `policyHash`.                                                                                                                | Repository search of `packages/*/src`      |
| Authorization binds authority identifier and version                                                     | Not found. No source file matches `authorityVersion` or `grantVersion`.                                                                                                                            | Repository search                          |
| Nonce or replay protection value                                                                         | Present. The term appears in 62 source files. Whether it is bound to the authorization is not yet confirmed.                                                                                       | Repository search                          |
| Authorization timestamp and expiry                                                                       | Present. The terms appear in 63 source files.                                                                                                                                                      | Repository search                          |
| Canonical action hash                                                                                    | Partly present. Action or intent hash terms appear in 19 source files.                                                                                                                             | Repository search                          |
| Unique transaction identifier                                                                            | Partly present. Transaction id terms appear in 7 source files.                                                                                                                                     | Repository search                          |
| Execution receipt carrying the executed action hash                                                      | Not found. No receipt term in `execution-gateway` or `execution-control`.                                                                                                                          | Repository search                          |
| Frozen cryptographic integrity core, which must not be modified                                          | No document defines a frozen core. The word appears only in one test file, `packages/crypto/tests/unit/crypto-verifiers.exact.test.ts`. The rule needs an owner decision and a written definition. | Repository search                          |
| Attack examples 35–46 and 78 run in CI                                                                   | Not confirmed. No CI reference found. Same open item as Section 2.                                                                                                                                 | Repository search of `.github` and `tests` |
| Mutation testing on evidence and records                                                                 | Not implemented. Stryker mutates source code only. An artifact-level harness does not exist yet.                                                                                                   | `stryker.config.mjs`; Section 4.1          |
| Crypto tests for policy change, execution intent and verification authorization                          | Present: `policy-change-crypto.test.ts`, `execution-intent-crypto.test.ts`, `verification-crypto-authorization.test.ts`. Coverage of T03 and T07 not reviewed.                                     | Repository file list                       |

### 12.1a Correction (14 November 2026)

Two items in 12.1 were wrong. A later search found the following.

- **Policy binding exists.** The authorization carries `policyContentHash`, and the gateway checks it against the live policy before release. The earlier search missed it because it looked for `policyHash`. XM-03 is therefore testable now.
- **Authority identifier exists.** The authorization carries `authorityGrant.grantId`. Only the version is missing.

Also, a signed Receipt already exists, and so does a connector evidence hash. The gap is an exact comparison between the authorized content hash and the dispatched content hash. The connector hash is computed over a redacted summary, so it can't be compared exactly. See the execution receipt design (moved to parmana-poae-benchmark).

### 12.2 Consequences for the plan

- POAE-03 (execution correspondence) cannot be tested until an execution receipt binding the executed action hash exists. That is a product gap, not only a testing gap.
- POAE-02 and T03 (policy change after authorization) need the policy-hash binding. This is the same open decision as Section 9, item 3.
- T10 (evidence removed from the ledger, detected by independent reconciliation) has no reconciliation component in the repository yet. Two candidate sources exist: the policy-log drift workflow and the caller audit chain. Neither has been reviewed for this purpose.
- The proposal's "frozen core" rule cannot be enforced until it is defined in writing.

### 12.3 Decisions (items 1 to 3 decided 14 November 2026, see D6 to D8)

1. Decided (D6): a separate research track, sharing the Phase 2 authorization tests.
2. Define the "frozen cryptographic integrity core" in writing, with the files it covers, before it is relied on.
3. Is a new execution receipt in scope? It is a product change and needs its own decision.
4. Approve the `parmana-poae-benchmark` structure only after items 1 to 3 are decided. Its experiments map to the existing phases, not new ones.
5. The 20-test benchmark: the category counts sum to 20, and the success criteria are targets only. Results must be reported as measured, including false rejections.

### 12.4 Scope of the research claim

The proposal correctly separates integrity from authenticity. A valid hash proves integrity relative to the hashed data, not that the event occurred. A valid signature proves that a key signed, not that the key holder had authority. Any PoAE paper must state both limits.
