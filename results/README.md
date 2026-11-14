# Results

Each run writes one JSON record per scenario, following `result-record.schema.json`. Records are kept here once a run has been reviewed.

Rules:

- `outcome` is `passed` only when the observed result matches the expected result in `scenarios/definitions.md`.
- A scenario that did not run is `not_run`, with the reason in `observed`. It is never counted as passed.
- `not_implemented` is for scenarios with no test yet.
- The record names the `parmana-core` commit tested. Results against different commits are not compared.
- Failures are recorded, not removed.

No records are stored yet. The first run is not recorded here.
