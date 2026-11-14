# Policy fixtures

Policy: `customer-refund`, copied from `parmana-core` at commit `d7082f0c`. The copies keep the benchmark independent of later edits to the live policies.

| Version | File                                | Content hash                                                       |
| ------- | ----------------------------------- | ------------------------------------------------------------------ |
| 1.2.0   | `customer-refund/1.2.0/policy.json` | `2a3a9534f0fe15d2d8e1a58b80044a09e78925c1849841af4fc2a6cc7ae78495` |
| 1.3.0   | `customer-refund/1.3.0/policy.json` | `53ce5abcacaf9cb749bfa78adf5d3e3ec7ff2d512b858a3612e11a15aa20c6e3` |

Hashes are computed with `TrustRecordHasher` from `@parmana/crypto`, with `PARMANA_POLICY_DIR=./policies`, `KEY_PROVIDER=local`, `PRIMARY_SIGNATURE_PROVIDER=ed25519` and `NODE_ENV=test`, under Node 24.22.2.

The two versions give the XM-03 case a real pair: an authorization bound to 1.2.0, presented after the live policy becomes 1.3.0, must be refused. If a file changes, its hash changes, and every expected value that depends on it must be regenerated in a reviewed change.
