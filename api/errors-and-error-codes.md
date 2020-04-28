---
description: Learn how to handle errors.
---

# Errors and Error Codes

Upon error, a response will contain details to help resolve the error. The response will contain the following fields:

### Error

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Code | `string` | Error code. |
| Message | `string` | A detailed error message. We recommend not to rely on the message to programmatically handle the error. Use Code to do that instead. |
| ID | `string` | **Optional.** For 500 responses only. If you encounter an error and this field is present, you can send us this ID for our reference. |
| ExtraData | `string` | _**Optional.**_ Extra data that may be helpful to handle the error. This could be a JSON string. |
| InnerErrors | `array of InnerError` | _**Optional.**_ For validation errors specifically. See [InnerError](errors-and-error-codes.md#innererror).  |

### InnerError

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Code | `string` | Error code. |
| Path | `string` | The name of the request object's attribute that has errors. |
| Message | `string` | A detailed error message. We recommend not to rely on the message to programmatically handle the error. Use Code to do that instead. |

