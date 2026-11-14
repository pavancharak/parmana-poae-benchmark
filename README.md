# parmana-poae-benchmark

**Proof of Authorized Execution (PoAE)**: the research programme for independently verifiable authorization of autonomous execution in Parmana.

## Why this repo exists

Autonomous systems act on behalf of a business, and today their own logs and credentials are the only record of what they did. This repository holds the research on whether an outside party can check that an action stayed within declared authority, even when parts of the infrastructure are compromised. It keeps research material apart from the production code in `parmana-core`.

## What is here

| Path | Contents |
| --- | --- |
| `specification/` | Phase 1 research specification: questions, hypotheses H1 to H6, properties P1 to P7, threat model, invariants, methodology, architecture, roadmap |
| `scenarios/` | The 30 scenarios as machine-readable JSON, with their status |
| `benchmark-spec.md` | The 20-test subset currently run, with its results |
| `EXECUTION_RECEIPT_DESIGN.md` | Design of the execution correspondence record |

## What is not here, and why

- **The benchmark runner** (`tests/assurance/poae/poae-benchmark.test.ts`) stays in `parmana-core`. It runs in that repo's CI gate and imports its packages. Copying it here would give two versions that drift apart.
- **Results** are recorded in `parmana-core`'s results log (`docs/ASSURANCE_RESULTS.md`), against the commit that was tested.

## How the work proceeds

Eight phases, each ending at a gate. The full plan is in `specification/research-roadmap.md`. Phase 1 is in review (gate G1). The next step after G1 is Phase 2: the scenario definitions, which exist here as `scenarios/scenarios.json`.

## Principles

1. Build on the existing Parmana code. Do not change the frozen cryptographic core.
2. Write each claim with the result that would falsify it.
3. Report failures and scenarios that cannot run. Never count them as passed.
4. Record the commit, the environment and the raw data for every result.
5. State what each claim does not establish: integrity, authenticity, authority, execution and completeness are separate.
