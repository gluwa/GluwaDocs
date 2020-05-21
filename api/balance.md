---
description: Retrieve the current balance of an address
---

# Balance

## `GET /v1/:currency/Addresses/:address`

Retrieve the current balance of an address.

### Request

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| currency | `string` | The [currency](currency-and-conversion-symbols.md#currency-symbols) unit of the balance |
| address | `string` | A string representing how much balance in the currency unit \(e.g., `"1.23"` for 1.23 USD-G\). |

#### Query Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| includeUnspentOutputs | `boolean` | _**Optional.**_ Unspent transaction outputs of the address. Available only for BTC addresses and if`includeUnspentOutputs` is set to `true`. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | [Balance](balance.md#balance) |

#### Balance

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Balance | `string` | A string representing how much balance in the currency unit \(e.g., `"1.23"` for 1.23 USD-G\). |
| Currency | `string` | The [currency](currency-and-conversion-symbols.md#currency-symbols) unit of the balance |
| UnspentOutputs | `array of` [`UnspentOutput`](balance.md#unspentoutput)\`\` | Unspent transaction outputs of the address. Available only for BTC addresses and if`includeUnspentOutputs` is set to `true`. |

#### UnspentOutput

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Amount | `string` | Bitcoin amount in the output index. |
| TxHash | `string` | Hash of the transaction that contains the output. |
| Index | `int` | Index of the output. |
| Confirmations | `int` | The number of confirmations for the transaction |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `BadRequest` | Invalid address format. |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 500 | `InternalServerError` | Server error. |
| 503 | `ServiceUnavailable` | Service unavailable for the specified currency |

