# Archived PoAE results (moved from parmana-core on 14 November 2026)

These entries were recorded in `parmana-core`'s results log against the commits named in each entry. The runner they describe is in `runner/poae-benchmark.test.ts`. The entries are copied unchanged. Commits named here exist in `parmana-core`'s history, not in this repository.

---

### 2026-11-14: PoAE benchmark runner (implemented tests)

- **Command:** `npm run build:packages`, then `node_modules/vitest/vitest.mjs run tests/assurance/poae` under Node 24.22.2 (`npx -y node@24.22.2`)
- **Runtime:** Node 24.22.2, vitest 5.1.0, Linux container. Test duration 1.13 s.
- **Commit tested:** `d7b4e48dd739e5868db738b6255494218a64bc34`
- **Branch:** `claude/lucid-sagan-1gs4xj`
- **Run by:** session (Claude Code), at the owner's direction

**Results**

| Measurement                                        |     Value |
| -------------------------------------------------- | --------: |
| Benchmark tests in the specification               |        20 |
| Tests that can run now (the specification's count) |        18 |
| Tests implemented in this runner                   |        11 |
| Passed                                             |        10 |
| Failed                                             | 1 (CE-02) |
| Not run                                            |         9 |

Passed: AM-01, XM-01, XM-02, RT-01, RT-02, RT-03, CE-03, EM-01 (record level), EM-02 (record level), EM-04 (record level).

**Failed: CE-02.** Altered signature bytes were expected to be rejected. Changing the final base64 character of an Ed25519 signature gave a different string that still verified. The signature bytes are the same, so this is not a forgery. The verifier nevertheless accepts more than one encoding of one signature. The test is kept failing and is not skipped.

**Checks not run (9):**

- AM-02: revocation registry not exercised by this runner.
- AM-03: unauthenticated authority source not modelled in the fixtures.
- AM-04: authority version binding absent (gap G-A).
- XM-03: policy change after authorization needs the policy binding path, which the legacy fixture bypasses.
- XM-04: fault injection for the authorization service not implemented.
- EM-03: direct execution bypassing the gateway not implemented.
- RT-04: agent identity binding not modelled in the fixtures.
- CE-01: signed trust-record tampering not implemented.
- CE-04: ledger reconciliation absent (gap G-L).

**Limits of these results:**

- XM-01 and XM-02 test the content-hash check. They do not test a policy limit or an approved vendor list.
- EM-01, EM-02 and EM-04 test the record-level correspondence check. They do not test the connector's outbound request.
- The fixtures use the legacy `allowUnverifiedPolicy` path.
- One run only. Results have not been repeated.

**Survivor classification:** not applicable. This entry records no mutation run.

**Open items**

1. CE-02 failure: decide whether signature verification must reject non-canonical base64. This changes the verifier, so it needs an owner decision, and it may fall under the frozen-core rule (D8, not yet defined).
2. Implement the seven testable tests not yet in the runner: AM-02, AM-03, XM-03, XM-04, EM-03, CE-01, RT-04.
3. XM-03 must run with the policy binding enabled, not on the legacy path.
4. The connector-level hash, which the record-level results do not cover.
5. CE-04 needs ledger reconciliation (gap G-L).

**Phase status change:** none. The benchmark is a research track (decision D6), not a plan phase.

---


### 2026-11-14: PoAE benchmark re-run after decision D9

- **Command:** `npm run build:packages`, then `node_modules/vitest/vitest.mjs run tests/assurance/poae`, then the full suite `node_modules/vitest/vitest.mjs run`. Both under Node 24.22.2.
- **Runtime:** Node 24.22.2, vitest 5.1.0, Linux container. Full suite 102.70 s as reported by vitest.
- **Commit tested:** `035e1697715da917899eaf4cb7112a5fb4393020`
- **Branch:** `claude/lucid-sagan-1gs4xj`
- **Run by:** session (Claude Code), at the owner's direction

**Results**

| Measurement                                 |                                                      Value |
| ------------------------------------------- | ---------------------------------------------------------: |
| PoAE benchmark tests implemented            |                                                         11 |
| PoAE benchmark: passed                      |                                                         11 |
| PoAE benchmark: failed                      |                                                          0 |
| Not run (no change from the previous entry) |                                                          9 |
| Full suite: test files passed               |                                           401 (29 skipped) |
| Full suite: tests passed                    | 3951 (106 skipped, 1 expected fail, from an existing test) |
| Full suite: failed                          |                                                          0 |

CE-02 now passes. The fix rejects non-canonical base64 signature encodings. The previous entry (commit `d7b4e48d`) recorded the failure and is unchanged.

**Limits:** unchanged from the previous entry. XM-01 and XM-02 test the content hash. EM tests are record-level. The legacy policy path is used. One run only.

**Survivor classification:** not applicable. No mutation run.

**Open items**

1. Implement the seven testable benchmark tests not yet in the runner: AM-02, AM-03, XM-03, XM-04, EM-03, CE-01, RT-04.
2. XM-03 must run with the policy binding enabled.
3. The connector-level hash, so the EM results cover the outbound request.
4. Ledger reconciliation for CE-04 (gap G-L).

**Phase status change:** none.

---


### 2026-11-14: PoAE benchmark, all implementable tests, and connector-level hash

- **Command:** `npm run build:packages`, then `node_modules/vitest/vitest.mjs run tests/assurance/poae`, then the full suite `node_modules/vitest/vitest.mjs run`. Both under Node 24.22.2.
- **Runtime:** Node 24.22.2, vitest 5.1.0, Linux container. Benchmark file 1.29 s.
- **Commit tested:** `bbe92321e32c9aed8dbc452dbb14f440fa66c43c`
- **Branch:** `claude/lucid-sagan-1gs4xj`
- **Run by:** session (Claude Code), at the owner's direction

**Results**

| Measurement                                                                   | Value |
| ----------------------------------------------------------------------------- | ----: |
| Benchmark tests in the specification                                          |    20 |
| Implemented in the runner and passed                                          |    18 |
| Failed                                                                        |     0 |
| Covered by an existing test, not run here (EM-03, architecture boundary test) |     1 |
| Gap, not implemented (AM-04, authority version binding absent)                |     1 |

The 18 passing tests: AM-01, AM-02, AM-03, XM-01, XM-02, XM-03, XM-04, RT-01, RT-02, RT-03, RT-04, CE-01, CE-02, CE-03, CE-04, EM-01 (record level), EM-02 (record level), EM-04 (record level). Each deny case has a paired allow case, so a system that always denies cannot pass.

Full suite on the same commit: a first run had one failure, an existing exact assertion on the HTTP connector's return value, which now expects the new `wireContentHash` field. That file passes after the fix in this commit. The first run's tallies were 3965 passed, 1 expected fail (existing), 106 skipped.

**Limits:**

- XM-01 and XM-02 test the content hash. They do not test a policy limit or an approved vendor list.
- EM-01, EM-02 and EM-04 test the record-level check. The HTTP connector now hashes the body it sends, so the dispatched hash for the generic connector reflects the outbound request. The vendor adapters are not covered.
- CE-04 tests the pure reconciliation function. The connection to the stored ledger is not implemented.
- One run, not repeated.

**Survivor classification:** not applicable. No mutation run.

**Open items**

1. Vendor adapters (`GatewayHttpAdapter`, `GatewayExternalAdapter`): their outbound requests are transformed from the content, so a hash over the request needs a mapping specification.
2. Connect the reconciliation to the stored ledger (gap G-L).
3. AM-04: authority version binding (gap G-A).
4. EM-03 is covered by the architecture boundary test. Run it as part of the benchmark report, not as a separate test.

**Phase status change:** none.

---

