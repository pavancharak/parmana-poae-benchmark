# Policy fixtures

`agent-vendor-payment/1.0.0/policy.json` is a copy of the policy of the same name in `parmana-core`, commit `d7082f0c`. It is copied so the benchmark does not change when the live policies change.

**Content hash** (computed with `TrustRecordHasher` from `@parmana/crypto`, `PARMANA_POLICY_DIR=./policies`, `KEY_PROVIDER=local`, `PRIMARY_SIGNATURE_PROVIDER=ed25519`, `NODE_ENV=test`, under Node 24.22.2):

```
f79e00d804b29ccc0f053bf52643afe6c477de293c567b054dc5656cecf4d200
```

An authorization bound to this policy carries this hash in `policyContentHash`. If the file changes, the hash changes, and every expected value that depends on it must be regenerated in a reviewed change.

Only one policy is fixed so far. A second version, for the XM-03 policy-change case, is still to be added.
