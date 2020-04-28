---
description: Retrieve the minimum fee amount for a transaction.
---

# Fee

A blockchain generally charges an optional fee to process a transaction \(e.g., [Bitcoin transaction fee](https://en.wikipedia.org/wiki/Bitcoin#Transaction_fees)\). This fee is optional, but a transaction with a low fee may not get processed by a blockchain at all. To prevent this, you generally want to pay some amount of fee for the transaction.

For Gluwacoin transactions \(eg&gt; USDG, KRWG, NGNG\), we charge extra fee on top of the market rate to ensure faster transaction. Any transaction with a fee lower than the minimum fee returned by the fee endpoint will be denied.

For Bitcoin transactions, we return the market rate fee. You may use this fee or use something lower, but please note that your transaction may take a while to be processed by the blockchain.

## `GET /v1/:currency/Fee`

Retrieve the minimum fee amount.

### Request

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| currency | `string` | The [currency](all-supported-currencies.md) unit for the fee. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | [Fee](fee.md#fee-1) |

#### Fee

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Currency | `string` | The [currency](all-supported-currencies.md) unit of the fee |
| MinimumFee | `string` | The minimum transaction fee for the currency |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 500 | `InternalServerError` | Server error. |
| 503 | `ServiceUnavailable` | Service unavailable for the specified currency |

