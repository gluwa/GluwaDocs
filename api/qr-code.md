---
description: Generate QR Code to receive payments.
---

# Payment QR Code

## `POST /v1/QRCode`

Retrieve QR Code for a payment.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| Authorization | `string` | Auth token using `Basic` scheme. See [API Keys and Secrets](authentication.md#api-keys-and-secrets). |

#### Query Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| format | `string` | _**Optional.**_ Image format, `image/png` or `image/jpeg`. If not specified, the API returns Base64 encoded string. |

#### Request Body

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Target | `string` | The address of the payment receiver. |
| Signature | `string` | Address Signature of the `Target` value, generate in the same way as [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |
| Currency | `string` | The [currency](currency-and-conversion-symbols.md#currency-symbols) of the payment. Not supported for `BTC`. |
| Amount | `string` | Payment amount. |
| MerchantOrderID | `string` | _**Optional.**_ A string value attached to the payment which can be used for traceability between Gluwa and your application. |
| Note | `string` | _**Optional.**_ Optional memo attached to the transaction. |
| Expiry | `int` | _**Optional.**_ The lifetime of the QR code in seconds. By default, the QR code will expire in 10 minutes. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | Base64 encoded image or `.png` or `.jpeg` file. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `MissingBody` | Request body is missing. |
| 400 | `InvalidBody` | Request validation errors. |
| 400 | `ValidationError` | Request validation errors. |
| 400 | `BadRequest` | Unsupported `format` query parameter value. |
| 403 | `Forbidden` | Not authorized to use this endpoint. Make sure your authorization header is correct and you are using valid API key and secret. |
| 500 | `InternalServerError` | Server error. |
| 503 | `ServiceUnavailable` | Service unavailable for the specified currency. |

