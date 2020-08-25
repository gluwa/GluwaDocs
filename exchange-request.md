# Exchange Request

## `PATCH /V1/ExchangeRequests/{ID}`

Accept an exchange request.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| Authorization | `string` | Auth token using `Basic` scheme. See [API Keys and Secrets](api/authentication.md#api-keys-and-secrets). |

#### Request Body

| Attribute | Type | Description |
| :--- | :--- | :--- |
| SendingAddress | `string` | The address that funds the source amount. Must be the same value as the `SendingAddress` you used when creating a new order. |
| ReserveTxnSignature | `string` | Reserve transaction signature used to reserve funds for the exchange. |
| Nonce | `int` | _**Optional**_. Required if the source currency is Gluwacoin currency \(ex&gt; `USD-G`, `KRW-G`\). Nonce is an integer used when creating reserve transaction signature. It must increase each time you make any new transactions \(transfer, exchange, etc\). |
| ExecuteTxnSignature | `string` | _**Optional.**_ Required if the source currency `BTC`. Execute transaction signature used to execute the exchange when your funds are available in the reserve address. |
| ReclaimTxnSignature | `string` | _**Optional.**_ Required if the source currency `BTC`. Reclaim transaction signature used to return the funds in the reserve address when the exchange fails or expires. |

### Response

| HTTP Status |
| :--- |
| 202 |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `MissingBody` | Request body is missing. |
| 400 | `InvalidBody` | Request validation errors. See InnerErrors. |
| 403 | `Forbidden` | Not authorized to use this endpoint. Make sure your authorization header is correct and you are using valid API key and secret. |
| 404 | `NotFound` | Exchange Request not found. |
| 409 | `Conflict` | Exchange Request cannot be accepted because it already has been accepted or failed. |
| 500 | `InternalServerError` | Server error. |

