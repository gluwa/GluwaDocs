# Order

## `GET v1/Orders`

Retrieve all orders.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| Authorization | `string` | Auth token using `Basic` scheme. See [API Keys and Secrets](authentication.md#api-keys-and-secrets). |

#### Query Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| startDateTime | `datetime` | ISO 8601 format datetime. If defined, only orders created after this datetime are included in the response. |
| endDateTime | `datetime` | ISO 8601 format datetime. If defined, only orders created before this datetime are included in the response. |
| status | `string` | `Active`, `Complete` or `Canceled`. If specified, only orders with the specified status will be included in the response. |
| offset | `int` | Number of orders to skip the beginning of list. Defaults to 0. |
| limit | `int` | Number of orders to include in the result. Defaults to 25, maximum of 100. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | array of [Orders](order.md#order). |

#### Order

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `string` | The order ID. |
| Conversion | `string` | Conversion symbol for the order. See [conversion](currency-and-conversion-symbols.md#conversion-symbols).  |
| SendingAddress | `string` | The address where the source amount is sent from. |
| SourceAmount | `string` | The amount available for exchange in source currency. |
| Price | `string` | The price the order will use for the exchange. The unit is `<exchanged currency>/<source currency>`. |
| ReceivingAddress | `string` | The address where the exchanged amount is received. |
| Status | `string` | `Active`, `Complete` or `Canceled`. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 403 | `Forbidden` | Not authorized to use this endpoint. Make sure your authorization header is correct and you are using valid API key and secret. |
| 500 | `InternalServerError` | Server error. |

## `GET v1/Orders/:ID`

Retrieve an order with specified ID.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| Authorization | `string` | Auth token using `Basic` scheme. See [API Keys and Secrets](authentication.md#api-keys-and-secrets). |

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `UUID` | Order ID. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | [Order](order.md#order-1). |

#### Order

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `string` | The order ID. |
| Conversion | `string` | Conversion symbol for the order. See [conversion](currency-and-conversion-symbols.md#conversion-symbols).  |
| SendingAddress | `string` | The address where the source amount is sent from. |
| SourceAmount | `string` | The amount available for exchange in source currency. |
| Price | `string` | The price the order will use for the exchange. The unit is `<exchanged currency>/<source currency>`. |
| ReceivingAddress | `string` | The address where the exchanged amount is received. |
| Status | `string` | `Active`, `Complete` or `Canceled`. |
| Exchanges | `array of` [`Exchanges`](order.md#exchange)\`\` | The list of the past and pending exchanges this order is fulfilling. |

#### Exchange

| Attribute | Type | Description |
| :--- | :--- | :--- |
| SendingAddress | `string` | The address where the source amount is sent from. |
| ReceivingAddress | `string` | The address where the exchanged amount is received. |
| SourceAmount | `string` | The amount in source currency to be exchanged. |
| Fee | `string` | The total fee paid for the exchange. |
| ExchangedAmount | `string` | The amount in exchanged currency you received. |
| Price | `string` | The price used for this exchange. The unit is `<exchanged currency>/<source currency>`. |
| Status | `string` | `Pending`, `Success` or `Failed`. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 403 | `Forbidden` | Access is denied for this resource. Make sure your authorization header is correct and you are using valid API key and secret |
| 404 | `NotFound` | Order not found. |
| 500 | `InternalServerError` | Server error. |

## `POST v1/Orders`

Create a new order.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| Authorization | `string` | Auth token using `Basic` scheme. See [API Keys and Secrets](authentication.md#api-keys-and-secrets). |

#### Request Body

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Conversion | `string` | Conversion symbol for the order. See [conversion](currency-and-conversion-symbols.md#conversion-symbols).  |
| SendingAddress | `string` | The address that funds the source amount. |
| SendingAddressSignature | `string` | Address Signature of the `SendingAddress` , generate in the same way as [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |
| ReceivingAddress | `string` | The address that the exchanged amount will be received. |
| ReceivingAddressSignature | `string` | Address Signature of the `ReceivingAddress` , generate in the same way as [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |
| SourceAmount | `string` | The amount in source currency to be exchanged. |
| Price | `string` | The price you want to use. Any exchange that this order will fulfill will use this price. The unit is `<exchanged currency>/<source currency>`. |
| BtcPublicKey | `string` | _**Optional.**_ Required if the source currency is `BTC`.  The public key for the sending address. Note that this is different from the public address. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 201 | [CreatedOrder](order.md#createdorder). |

#### CreatedOrder

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `UUID` | The order ID. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `MissingBody` | Request body is missing. |
| 400 | `InvalidBody` | Request validation errors. See InnerErrors. |
| 400 | `ValidationError` | Request validation errors. See InnerErrors. |
| 403 | `Forbidden` | Not authorized to use this endpoint. Make sure your authorization header is correct and you are using valid API key and secret. |
| 403 | `WebhookNotFound` | Webhook URL to send exchange request is unavailable. |
| 500 | `InternalServerError` | Server error. |
| 503 | `ServiceUnavailable` | Service unavailable for the specified conversion. |

## `PATCH v1/Orders/:ID`

Cancel an order.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| Authorization | `string` | Auth token using `Basic` scheme. See [API Keys and Secrets](authentication.md#api-keys-and-secrets). |

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `UUID` | Order ID. |

#### Request Body

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Action | `string` | Use `Cancel`. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | none. If the order is already canceled, it will have no effect and return 200. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `MissingBody` | Request body is missing. |
| 400 | `InvalidBody` | Request validation errors. See InnerErrors. |
| 400 | `ValidationError` | Request validation errors. See InnerErrors. |
| 403 | `Forbidden` | Access is denied for this resource. Make sure your authorization header is correct and you are using valid API key and secret |
| 404 | `NotFound` | Order is not found. |
| 409 | `Conflict` | Cannot be canceled. Usually because the order is already completed. |
| 500 | `InternalServerError` | Server error. |

