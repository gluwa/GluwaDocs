---
description: Create and retrieve transactions.
---

# Transaction

## `GET /v1/:currency/Addresses/:address/Transactions`

Get transaction history for a given address.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| X-REQUEST-SIGNATURE | `string` | Address Signature of `address` path parameter. See [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| currency | `string` | The [currency](all-supported-currencies.md) of the transactions. |
| address | `string` | The public address associated with the transactions. |

#### Query Parameters

<table>
  <thead>
    <tr>
      <th style="text-align:left">Attribute</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">status</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p><em><b>Optional.</b></em> Either <code>Confirmed</code> or <code>Incomplete</code>.
          <br
          />
        </p>
        <p>Confirmed (default) - Only includes confirmed transactions on the blockchain.</p>
        <p>Incomplete - Only includes unconfirmed and failed transactions in Gluwa.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">offset</td>
      <td style="text-align:left"><code>int</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em> The number of entries to skip.</td>
    </tr>
    <tr>
      <td style="text-align:left">limit</td>
      <td style="text-align:left"><code>int</code>
      </td>
      <td style="text-align:left"><em><b>Optional. </b></em>The number of transactions returned in in the
        response (max 100).</td>
    </tr>
  </tbody>
</table>### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | array of [Transaction](transaction.md#transaction-1). |

#### Transaction

<table>
  <thead>
    <tr>
      <th style="text-align:left">Attribute</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Status</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>The status of the transaction.<code>Unconfirmed</code>, <code>Confirmed</code>,
          or <code>Failed</code>.
          <br />
        </p>
        <p>Unconfirmed - The transaction was announce to the blockchain, but is not
          included in any block yet.
          <br />Confirmed - The transaction was included in the blockchain and received
          a confirmation. Failed - The transaction has failed for some reason.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Amount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The amount sent or received in this transaction.</td>
    </tr>
    <tr>
      <td style="text-align:left">TotalAmount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The sum of the <code>Amount</code> and the <code>Fee</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">Currency</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The <a href="all-supported-currencies.md">currency</a> of the transaction</td>
    </tr>
    <tr>
      <td style="text-align:left">Sources</td>
      <td style="text-align:left"><code>array of string</code>
      </td>
      <td style="text-align:left">The sources of the transaction. For BTC, there can be multiple sources.</td>
    </tr>
    <tr>
      <td style="text-align:left">Targets</td>
      <td style="text-align:left"><code>array of string</code>
      </td>
      <td style="text-align:left">The targets of the transaction. For BTC, there can be multiple targets.</td>
    </tr>
    <tr>
      <td style="text-align:left">TxnHash</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Blockchain transaction hash.</td>
    </tr>
    <tr>
      <td style="text-align:left">CreatedDateTime</td>
      <td style="text-align:left"><code>datetime</code>
      </td>
      <td style="text-align:left">Time at which the transaction was created.</td>
    </tr>
    <tr>
      <td style="text-align:left">ModifiedDateTime</td>
      <td style="text-align:left"><code>datetime</code>
      </td>
      <td style="text-align:left">Time at which the transaction was last modified.</td>
    </tr>
    <tr>
      <td style="text-align:left">MerchantOrderID</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em><b> </b>Used by the receiver to identify a payment.
        Supported by QR code payment feature only.</td>
    </tr>
    <tr>
      <td style="text-align:left">Note</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em> A message attached to the transaction. It is
        an optional memo you can associate with the transaction.</td>
    </tr>
    <tr>
      <td style="text-align:left">Fee</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em> Transaction fee.</td>
    </tr>
  </tbody>
</table>### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `BadRequest` | Invalid address format. |
| 403 | `SignatureMissing` | `X-REQUEST-SIGNATURE` header is missing. |
| 403 | `SignatureExpired` | `X-REQUEST-SIGNATURE` has expired. |
| 403 | `InvalidSignature` | Invalid `X-REQUEST-SIGNATURE`. |
| 500 | `InternalServerError` | Server error. |
| 503 | `ServiceUnavailable` | Service unavailable for the specified currency |

## `GET /v1/:currency/Transactions/:txnhash`

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| X-REQUEST-SIGNATURE | `string` | Address Signature of an address involved with `txnhash`. See [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| currency | `string` | The [currency](all-supported-currencies.md) of the transaction. |
| txnhash | `string` | Blockchain transaction hash. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | [Transaction](transaction.md#transaction-1). |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `BadRequest` | Invalid `txnhash` value. |
| 403 | `SignatureMissing` | `X-REQUEST-SIGNATURE` header is missing. |
| 403 | `SignatureExpired` | `X-REQUEST-SIGNATURE` has expired. |
| 403 | `InvalidSignature` | Invalid `X-REQUEST-SIGNATURE`. |
| 404 | `NotFound` | Transaction not found. |
| 500 | `InternalServerError` | Server error. |
| 503 | `ServiceUnavailable` | Service unavailable for the specified currency |

## `POST /v1/Transactions`

### Request

#### Request Body

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Signature | `string` | Transaction signed with the sender's private key. See [Creating Transaction Signatures](../development/todo-creating-transaction-signatures.md#transaction-signature). |
| Amount | `string` | Transaction amount, not including the fee. This is the amount that the receiver receives. |
| Fee | `string` | Transaction fee amount. Generally, you should use the amount from the [Fee endpoint](fee.md#get-v-1-currency-fee). Your transaction may not get processed by Gluwa if the fee amount is smaller than the minimum fee amount. |
| Currency | `string` | The [currency](all-supported-currencies.md) of the transaction. |
| Source | `string` | Address of the sender. |
| Target | `string` | Address of the receiver. |
| Nonce | `string` | _**Optional.**_ Required if using Gluwacoin as the currency of the transaction. Nonce is an integer used when creating `Signature`. It must increase each time you make a transaction. |
| MerchantOrderID | `string` | _**Optional.**_ A string value attached to the transaction which can be used for traceability between Gluwa and your application. |
| Note | `string` | _**Optional.**_ Optional memo attached to the transaction. |
| Idem | `UUID` | _**Optional.**_ Used for idempotent requests. See [Idempotent Requests](../development/todo-idempotent-requests.md). |
| PaymentID | `UUID` | _**Optional.**_ A unique identifier for a payment used with QR code payment feature. You can get this value by decoding the QR code. |
| PaymentSig | `string` | _**Optional.**_ Required if `PaymentID` is not null. This is provided to you when you use [QR Code](qr-code.md) endpoint. You can get this value by decoding the QR code. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 202 | _**Optional.**_ [TransactionHash](transaction.md#transactionhash). Only for `BTC` transactions. |

#### TransactionHash

| Attribute | Type | Description |
| :--- | :--- | :--- |
| TxnHash | `string` | `BTC` transaction hash. For Gluwacoin transactions, the transaction hash is not immediately available, so it is not included in the response. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `MissingBody` | Request body is missing. |
| 400 | `InvalidBody` | Request validation errors. |
| 400 | `ValidationError` | Request validation errors. |
| 403 | `InvalidPaymentSignature` | Invalid payment signature. |
| 409 | `Conflict` | Transfer already exists. |
| 500 | `InternalServerError` | Server error. |
| 503 | `ServiceUnavailable` | Service unavailable for the specified currency |

