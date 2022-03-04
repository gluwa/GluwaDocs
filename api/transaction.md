---
description: Create and retrieve transactions.
---

# Transaction

## `GET /v1/:currency/Addresses/:address/Transactions`

Get transaction history for a given address.

### Request

#### Headers

| Header              | Type     | Description                                                                                                      |
| ------------------- | -------- | ---------------------------------------------------------------------------------------------------------------- |
| X-REQUEST-SIGNATURE | `string` | Address Signature of `address` path parameter. See [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |

#### Path Parameters

| Attribute | Type     | Description                                                                              |
| --------- | -------- | ---------------------------------------------------------------------------------------- |
| currency  | `string` | The [currency](currency-and-conversion-symbols.md#currency-symbols) of the transactions. |
| address   | `string` | The public address associated with the transactions.                                     |

#### Query Parameters

| Attribute | Type     | Description                                                                                                                                                                                                                                                                 |
| --------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| status    | `string` | <p><em><strong>Optional.</strong></em> Either <code>Confirmed</code> or <code>Incomplete</code>.<br></p><p>Confirmed (default) - Only includes confirmed transactions on the blockchain.</p><p>Incomplete - Only includes unconfirmed and failed transactions in Gluwa.</p> |
| offset    | `int`    | _**Optional.**_ The number of entries to skip.                                                                                                                                                                                                                              |
| limit     | `int`    | _**Optional.**_ The number of transactions returned in in the response (max 100).                                                                                                                                                                                           |

### Response

| HTTP Status | Return Object                                       |
| ----------- | --------------------------------------------------- |
| 200         | array of [Transaction](transaction.md#transaction). |

#### Transaction

| Attribute        | Type              | Description                                                                                                                                                                                                                                                                                                                                                                  |
| ---------------- | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Status           | `string`          | <p>The status of the transaction.<code>Unconfirmed</code>, <code>Confirmed</code>, or <code>Failed</code>.<br></p><p>Unconfirmed - The transaction was announce to the blockchain, but is not included in any block yet.<br>Confirmed - The transaction was included in the blockchain and received a confirmation. Failed - The transaction has failed for some reason.</p> |
| Amount           | `string`          | The amount sent or received in this transaction.                                                                                                                                                                                                                                                                                                                             |
| TotalAmount      | `string`          | The sum of the `Amount` and the `Fee`.                                                                                                                                                                                                                                                                                                                                       |
| Currency         | `string`          | The [currency](currency-and-conversion-symbols.md#currency-symbols) of the transaction                                                                                                                                                                                                                                                                                       |
| Sources          | `array of string` | The sources of the transaction. For BTC, there can be multiple sources.                                                                                                                                                                                                                                                                                                      |
| Targets          | `array of string` | The targets of the transaction. For BTC, there can be multiple targets.                                                                                                                                                                                                                                                                                                      |
| TxnHash          | `string`          | Blockchain transaction hash.                                                                                                                                                                                                                                                                                                                                                 |
| CreatedDateTime  | `datetime`        | Time at which the transaction was created.                                                                                                                                                                                                                                                                                                                                   |
| ModifiedDateTime | `datetime`        | Time at which the transaction was last modified.                                                                                                                                                                                                                                                                                                                             |
| MerchantOrderID  | `string`          | _**Optional.**_** ** Used by the receiver to identify a payment. Supported by QR code payment feature only.                                                                                                                                                                                                                                                                  |
| Note             | `string`          | _**Optional.**_  A message attached to the transaction. It is an optional memo you can associate with the transaction.                                                                                                                                                                                                                                                       |
| Fee              | `string`          | _**Optional.**_ Transaction fee.                                                                                                                                                                                                                                                                                                                                             |
| ID               | `UUID`            | _**Optional**_. Gluwa's internal transaction ID. If the transaction was made outside of Gluwa's system (ex> Transaction was made directly on the blockchain), then this will not be available.                                                                                                                                                                               |

### Errors

| HTTP Status | Error Code             | Description                                    |
| ----------- | ---------------------- | ---------------------------------------------- |
| 400         | `InvalidUrlParameters` | Invalid URL parameters                         |
| 400         | `BadRequest`           | Invalid address format.                        |
| 403         | `SignatureMissing`     | `X-REQUEST-SIGNATURE` header is missing.       |
| 403         | `SignatureExpired`     | `X-REQUEST-SIGNATURE` has expired.             |
| 403         | `InvalidSignature`     | Invalid `X-REQUEST-SIGNATURE`.                 |
| 500         | `InternalServerError`  | Server error.                                  |
| 503         | `ServiceUnavailable`   | Service unavailable for the specified currency |

## `GET /v1/:currency/Transactions/:txnhash`

### Request

#### Headers

| Header              | Type     | Description                                                                                                                |
| ------------------- | -------- | -------------------------------------------------------------------------------------------------------------------------- |
| X-REQUEST-SIGNATURE | `string` | Address Signature of an address involved with `txnhash`. See [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |

#### Path Parameters

| Attribute | Type     | Description                                                                             |
| --------- | -------- | --------------------------------------------------------------------------------------- |
| currency  | `string` | The [currency](currency-and-conversion-symbols.md#currency-symbols) of the transaction. |
| txnhash   | `string` | Blockchain transaction hash.                                                            |

### Response

| HTTP Status | Return Object                              |
| ----------- | ------------------------------------------ |
| 200         | [Transaction](transaction.md#transaction). |

### Errors

| HTTP Status | Error Code             | Description                                    |
| ----------- | ---------------------- | ---------------------------------------------- |
| 400         | `InvalidUrlParameters` | Invalid URL parameters                         |
| 400         | `BadRequest`           | Invalid `txnhash` value.                       |
| 403         | `SignatureMissing`     | `X-REQUEST-SIGNATURE` header is missing.       |
| 403         | `SignatureExpired`     | `X-REQUEST-SIGNATURE` has expired.             |
| 403         | `InvalidSignature`     | Invalid `X-REQUEST-SIGNATURE`.                 |
| 404         | `NotFound`             | Transaction not found.                         |
| 500         | `InternalServerError`  | Server error.                                  |
| 503         | `ServiceUnavailable`   | Service unavailable for the specified currency |

## `POST /v1/Transactions`

### Request

#### Request Body

| Attribute       | Type     | Description                                                                                                                                                                                                                                                                   |
| --------------- | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Signature       | `string` | Transaction signed with the sender's private key. See [Creating Transaction Signatures](../development/creating-transaction-signatures.md#transaction-signature).                                                                                                             |
| Amount          | `string` | Transaction amount, not including the fee. This is the amount that the receiver receives.                                                                                                                                                                                     |
| Fee             | `string` | Transaction fee amount. Generally, you should use the amount from the [Fee endpoint](fee.md#get-v-1-currency-fee). Your transaction may not get processed by Gluwa if the fee amount is smaller than the minimum fee amount.                                                  |
| Currency        | `string` | The [currency](currency-and-conversion-symbols.md#currency-symbols) of the transaction.                                                                                                                                                                                       |
| Source          | `string` | Address of the sender.                                                                                                                                                                                                                                                        |
| Target          | `string` | Address of the receiver.                                                                                                                                                                                                                                                      |
| Nonce           | `string` | _**Optional.**_ Required if using Gluwacoin as the currency of the transaction. Nonce is an unsigned integer used when creating reserve transaction signature. It must be unique each time you make any new transactions (transfer, exchange, etc). Maximum value is 2^256-1. |
| MerchantOrderID | `string` | _**Optional.**_ A string value attached to the transaction which can be used for traceability between Gluwa and your application.                                                                                                                                             |
| Note            | `string` | _**Optional.**_ Optional memo attached to the transaction.                                                                                                                                                                                                                    |
| Idem            | `UUID`   | _**Optional.**_ Used for idempotent requests. See [Idempotent Requests](../development/idempotent-requests.md).                                                                                                                                                               |
| PaymentID       | `UUID`   | _**Optional.**_ A unique identifier for a payment used with QR code payment feature. You can get this value by decoding the QR code.                                                                                                                                          |
| PaymentSig      | `string` | _**Optional.**_ Required if `PaymentID` is not null. This is provided to you when you use [QR Code](qr-code.md) endpoint. You can get this value by decoding the QR code.                                                                                                     |

### Response

| HTTP Status | Return Object                                                                                   |
| ----------- | ----------------------------------------------------------------------------------------------- |
| 202         | _**Optional.**_ [TransactionHash](transaction.md#transactionhash). Only for `BTC` transactions. |

#### TransactionHash

| Attribute | Type     | Description                                                                                                                                   |
| --------- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| TxnHash   | `string` | `BTC` transaction hash. For Gluwacoin transactions, the transaction hash is not immediately available, so it is not included in the response. |

### Errors

| HTTP Status | Error Code                | Description                                    |
| ----------- | ------------------------- | ---------------------------------------------- |
| 400         | `InvalidUrlParameters`    | Invalid URL parameters                         |
| 400         | `MissingBody`             | Request body is missing.                       |
| 400         | `InvalidBody`             | Request validation errors.                     |
| 400         | `ValidationError`         | Request validation errors.                     |
| 403         | `InvalidPaymentSignature` | Invalid payment signature.                     |
| 409         | `Conflict`                | Transfer already exists.                       |
| 500         | `InternalServerError`     | Server error.                                  |
| 503         | `ServiceUnavailable`      | Service unavailable for the specified currency |
