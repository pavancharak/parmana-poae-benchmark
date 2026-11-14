# parmana-poae-benchmark

**Proof of Authorized Execution (PoAE)**: a research programme testing whether an outside party can check that an autonomous action stayed within the authority a business declared, even when parts of the infrastructure are compromised.

**Owner:** Pavan Charak.
**Status:** Phase 1 (research specification) passed gate G1 on 14 November 2026, on owner approval. Phase 2 is under way. No benchmark result in this repository is final.

## 1. Objective

**Research question.** How can an organization independently establish that an autonomous system executed an action within explicitly declared business authority, when components of its infrastructure may be compromised?

**Central thesis.** An autonomous system should not be trusted on its own identity, credentials, operator assurances or self-reported logs. Its actions should be constrained by explicit authority, and supported by evidence that an independent verifier can check.

**Model.** Declared → Authorized → Executed → Proven.

| Stage      | Meaning                                                                    |
| ---------- | -------------------------------------------------------------------------- |
| Declared   | Business authority is explicit, authenticated and versioned                |
| Authorized | The requested action is evaluated deterministically against that authority |
| Executed   | Execution is constrained to the authorized action                          |
| Proven     | Independent verification establishes what the evidence supports            |

**Five research objectives.**

| ID  | Contribution                                                         |
| --- | -------------------------------------------------------------------- |
| R1  | Machine-verifiable representation of business authority              |
| R2  | Deterministic authorization enforcement                              |
| R3  | Correspondence between the authorized action and the executed action |
| R4  | Tamper-evident and complete execution evidence                       |
| R5  | Independent verification under adversarial conditions                |

**Out of scope.** Whether a real-world event happened. PoAE checks what was recorded, signed and verified. Confidentiality of transaction data is a later, optional extension.

## 2. Hypotheses

Each hypothesis states the result that would falsify it. A result that cannot falsify a claim is not a result.

| ID  | Claim                                                                                                                            | Falsified by                                                                                                      |
| --- | -------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| H1  | Deterministic authorization rejects every defined unauthorized action, and gives the same decision for the same input and policy | One unauthorized action approved, or two different decisions for one input                                        |
| H2  | Action binding detects any change between authorization and execution                                                            | A dispatched action that differs from the authorized action and passes the check                                  |
| H3  | Independent checkpoints detect evidence deletion and ledger truncation                                                           | A removed record or truncated ledger that is reported as passed                                                   |
| H4  | Revocation prevents later unauthorized execution, within a stated window                                                         | A release under a revoked grant or key after the window closes                                                    |
| H5  | Formal modelling finds unsafe transitions that conventional tests miss                                                           | Exhaustive checking within bounds finds nothing that the tests would not also have found (this is also a finding) |
| H6  | Privacy-preserving proofs reduce disclosure without losing verification                                                          | A non-compliant input that verifies, or a proof that discloses a value it should hide                             |

Full statements are in [`specification/research-hypotheses.md`](specification/research-hypotheses.md).

## 3. Tests

The threat model defines **30 adversarial scenarios**, T01 to T30. Each has an expected result and a property it tests. The machine-readable list is [`scenarios/scenarios.json`](scenarios/scenarios.json).

| Category                    | Scenarios                                                                                                                                                                                       |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Authority                   | T01 to T04: unauthenticated authority, expired authority, revoked grant, re-issued authority                                                                                                    |
| Authorization and execution | T05 to T10, T18: amount above limit, unauthorized target, policy changed after approval, service unavailable, direct execution, agent changing its own authority, grant reused by another agent |
| Cryptographic               | T11 to T15: altered record, altered signature, untrusted key, non-canonical encoding, broken hash chain                                                                                         |
| Replay and availability     | T16, T17, T19: double submission, nonce reuse, replay after expiry                                                                                                                              |
| Execution correspondence    | T20, T21, T28, T29: dispatched amount, dispatched target, dispatch hash omitted, connector request differs                                                                                      |
| Infrastructure compromise   | T22 to T27, T30: evidence removed, ledger truncated, key compromised, trust root substituted, policy store substituted, store write failure, late revocation                                    |

**Status on 14 November 2026:**

| Status          | Count | Meaning                                                                 |
| --------------- | ----: | ----------------------------------------------------------------------- |
| Implemented     |    18 | A test in the parmana-core runner passes                                |
| Elsewhere       |     2 | Covered by an existing test (architecture boundary; canonical encoding) |
| Gap             |     1 | The repository lacks the mechanism (T04: authority version)             |
| Partial         |     1 | Covered for one path only (T29: HTTP connector only)                    |
| Not implemented |     8 | No test yet                                                             |

The 18 implemented scenarios run in `parmana-core`, in `tests/assurance/poae/poae-benchmark.test.ts`, as part of its CI build. The runner stays there because it imports the product packages. Results are recorded in `parmana-core`'s results log, against the commit that was tested.

**What the tests do not show.** They show that the defined scenarios were refused or detected. They do not show that no other attack exists. Several implemented tests use limited versions of the scenario: T05 and T06 test the content hash, not a policy limit or an approved vendor list.

## 4. Approach

1. **Specify before building.** Questions, hypotheses, properties, assumptions, threats and invariants are written and reviewed first. Phase 2 started after gate G1 passed on 14 November 2026.
2. **Reuse the product.** The experiments run against Parmana's existing gateway, signed trust record, offline verifier and reconciliation function.
3. **Keep the frozen core frozen.** The signing, hashing and verification core is not changed to suit the research. Its state and change process are in `parmana-core`, at `docs/assurance/FROZEN_CORE.md`.
4. **Pair every deny with an allow.** A deny test is meaningful only when the same path also allows a legitimate action. A system that refuses everything would otherwise pass.
5. **Measure with stated denominators.** Each metric has its own denominator. A missing evidence record is never counted as a passed verification because the rest of the chain is valid.
6. **Compare against baselines.** A claim that PoAE adds something is tested against an application-level baseline (A) and a signed-log baseline (B), not only against nothing.
7. **Report failures.** A scenario that fails, or cannot run, is recorded as such. A failed gate is investigated, not relaxed.

The eight phases and their gates are in [`specification/research-roadmap.md`](specification/research-roadmap.md).

## 5. Why

**Why this exists.** Autonomous systems act for a business, and their own logs and credentials are often the only record of what they did. If that infrastructure is compromised, nobody outside can tell. PoAE asks whether that gap can be closed by evidence an outsider can check.

**Why research, not only product tests.** A passing test suite shows that the tests were not defeated by the scenarios they cover. It does not show that the claim holds generally. A research programme forces the claim to be stated, the threats enumerated, and the failures published.

**Why separate repositories.** Research claims need their own review, and their own record of what has and has not been shown. Keeping them apart from `parmana-core` stops a product release being read as evidence for a research claim, or the reverse.

**Why the frozen core.** If the cryptographic core were changed to make the research pass, the research would be testing the change, not the design. Freezing it keeps the result about the system as it is.

**Why the baselines.** Without them, a result such as "18 scenarios refused" says nothing about what PoAE adds over ordinary logging.

**Why report what we cannot yet show.** Eight scenarios are not implemented, one is a gap, and several limits apply to the implemented ones. Stating these is what makes the rest credible.

## 6. Current limits and open decisions

- **Vendor adapters:** the dispatched-content hash covers only the HTTP connector (decision D10, owner).
- **Ledger connection and external checkpoint:** reconciliation exists as a pure function, not yet connected to a ledger. Without an external checkpoint, deletion detection can be undone by whoever controls the database (decision D11).
- **Authority version:** not bound (decision D12, owner).
- **Policy change after approval:** the benchmark denies a changed policy. The owner has not yet ratified that behaviour.
- **Formal model, compromise tests, zero-knowledge work:** not started (Phases 4 to 6).

## 7. Files

| Path                                                         | Contents                                      |
| ------------------------------------------------------------ | --------------------------------------------- |
| [`specification/`](specification/README.md)                  | Phase 1 research specification                |
| [`scenarios/`](scenarios/README.md)                          | The 30 scenarios as JSON                      |
| [`benchmark-spec.md`](benchmark-spec.md)                     | The 20-test subset run to date, with results  |
| [`EXECUTION_RECEIPT_DESIGN.md`](EXECUTION_RECEIPT_DESIGN.md) | Design of the execution correspondence record |

## 8. Next step

Gate G1 passed on 14 November 2026. Phase 2 (benchmark foundation) is under way.

## 9. Moved from parmana-core (14 November 2026)

Everything PoAE that was in `parmana-core` now lives here:

| Path                                     | Moved from (in parmana-core, before commit 074895bd)                                    |
| ---------------------------------------- | --------------------------------------------------------------------------------------- |
| `runner/poae-benchmark.test.ts`          | `tests/assurance/poae/poae-benchmark.test.ts`                                           |
| `runner/scenarios.test.ts`               | `tests/assurance/poae/scenarios.test.ts` (last version, commit 51095929), paths adapted |
| `archive/plan-section-12-poae-review.md` | Section 12 of `docs/ASSURANCE_TESTING_PLAN.md`                                          |
| `archive/decision-D6.md`                 | Decision D6 in `docs/ASSURANCE_DECISIONS.md`                                            |
| `archive/results-poae-entries.md`        | The three PoAE benchmark entries in `docs/ASSURANCE_RESULTS.md`                         |

**The runner depends on parmana-core.** It imports the product packages from `packages/` in `parmana-core`, so it does not run from this repository alone. To run it, check out `parmana-core` and copy the runner to `tests/assurance/poae/` there. Its results are valid only for that checkout's commit. The copy here is a reference, not a second live test.

The benchmark is no longer part of `parmana-core`'s CI gate.
