# Esure Scenario Format

## Goals

The format is readable, deterministic, versioned, safe to validate, and
independent of frontend presentation. JSON is canonical; YAML may come later.

## Initial example

```json
{
  "schemaVersion": 1,
  "id": "issued-asset-payment",
  "name": "Issued asset payment",
  "description": "Issue TESTUSD and send it to a funded recipient.",
  "network": "testnet",
  "accounts": [
    { "id": "issuer", "fundWithXlm": "100" },
    { "id": "recipient", "fundWithXlm": "10" }
  ],
  "assets": [
    { "id": "test-usd", "code": "TESTUSD", "issuer": "issuer" }
  ],
  "steps": [
    {
      "id": "recipient-trusts-test-usd",
      "type": "changeTrust",
      "account": "recipient",
      "asset": "test-usd"
    },
    {
      "id": "issuer-pays-recipient",
      "type": "payment",
      "from": "issuer",
      "to": "recipient",
      "asset": "test-usd",
      "amount": "100"
    }
  ],
  "assertions": [
    {
      "type": "balanceEquals",
      "account": "recipient",
      "asset": "test-usd",
      "amount": "100"
    }
  ]
}
```

## MVP step types

### `changeTrust`

Creates or updates a classic asset trustline. Required fields are `account` and
`asset`; optional `limit` is a positive decimal string.

### `payment`

Sends XLM or an issued asset. Required fields are `from`, `to`, `asset`, and
`amount`.

## MVP assertion types

- `balanceEquals`: verifies an account balance after execution.
- `stepSucceeded`: verifies that a referenced step completed successfully.
- `stepFailedWith`: verifies an expected normalized Stellar result code.

## Validation rules

- `schemaVersion` equals `1`.
- IDs use lowercase letters, digits, and hyphens and are unique in their scope.
- `network` equals `testnet` in the MVP.
- Account and asset references resolve inside the scenario.
- Asset codes satisfy Stellar classic asset constraints.
- Amounts and limits are positive canonical decimal strings.
- Steps run sequentially in document order.
- Accounts, assets, steps, and total execution time are bounded.
- Unknown properties are rejected.
- Secret keys, arbitrary URLs, scripts, and raw envelopes are forbidden.

## Initial bundled scenarios

1. `xlm-payment`: fund two accounts, send XLM, and verify the balance change.
2. `issued-asset-payment`: establish a trustline, send `TESTUSD`, and verify it.
3. `missing-trustline`: omit the trustline and verify the expected failure.

## Versioning

Released scenarios are immutable. Breaking format changes increment
`schemaVersion`; changed bundled scenarios increment their catalog version.
Reports record both values for reproducibility.

