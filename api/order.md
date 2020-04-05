# Order

## The Order Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **ID** | `string` | `uuid` Order ID |
| **Conversion** | `string` | `enum ["UsdgKrwg", "KrwgUsdg"]` Conversion of the exchange. For example, `"UsdgKrwg"` is exchanging USD-G to KRW-G and `"KrwgUsdg"` is exchanging KRW-G to USD-G. |
| **SendingAddress** | `string` | The address where the amount in source currency is sent from. |
| **SourceAmount** | `string` | The amount to exchange |
| **Price** | `string` | Price per source currency |
| **ReceivingAddress** | `string` | The address where the exchanged currency is received. |
| **Status** | `string` | `enum ["Active", "Complete", "Canceled"]`The status of the order: `"Active"`, `"Complete"`, or `"Canceled"` |
| **Exchanges** | `array` | `array<Exchange>` List of exchanges that are performed for the order. |

{% code title="Example" %}
```javascript
{
  "ID": "string (uuid)",
  "Conversion": "string",
  "SendingAddress": "string",
  "SourceAmount": "string",
  "Price": "string",
  "ReceivingAddress": "string",
  "Status": "string",
  "Exchanges": [
    {
      "SendingAddress": "string",
      "ReceivingAddress": "string",
      "SourceAmount": "string",
      "ExchangedAmount": "string",
      "Price": "string",
      "Status": "string"
    }
  ]
}
```
{% endcode %}

## The Exchange Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **SendingAddress** | `string` | The address where the amount in source currency is sent from. |
| **ReceivingAddress** | `string` | The address where the exchanged currency is received. |
| **SourceAmount** | `string` | The amount of funds to be exchanged. |
| **ExchangedAmount** | `string` | The amount of exchanged funds you will receive |
| **Price** | `string` | Price per source currency used for this exchange |
| **Status** | `string` | `enum ["Active", "Complete", "Canceled"]` The status of the order: `"Pending"`, `"Success"`, or `"Failed"`. |

{% code title="Example" %}
```javascript
{
  "SendingAddress": "string",
  "ReceivingAddress": "string",
  "SourceAmount": "string",
  "ExchangedAmount": "string",
  "Price": "string",
  "Status": "string"
}
```
{% endcode %}

{% api-method method="post" host="https://api.gluwa.com" path="/V1/Orders" %}
{% api-method-summary %}
Create Order
{% endapi-method-summary %}

{% api-method-description %}
Create an order
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Gluwa uses API keys and basic access authentication to authenticate this endpoint. Refer to "API Keys" in the Authentication section for details.
{% endapi-method-parameter %}

{% api-method-parameter name="Content-Types" type="string" required=true %}
application/json-patch+json, application/json, text/json, application/\*+json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="Conversion" type="string" required=true %}
Conversion for the exchange, from source currency to exchanged currency. `"UsdgKrwg"` for converting USD-G to KRW-G and `"KrwgUsdg"` for converting KRW-G to USDG.
{% endapi-method-parameter %}

{% api-method-parameter name="SendingAddress" type="string" required=true %}
The address of the source.
{% endapi-method-parameter %}

{% api-method-parameter name="SendingAddressSignature" type="string" required=true %}
The signature of the sending address. Must be the same format as X-REQUEST-SIGNATURE header.
{% endapi-method-parameter %}

{% api-method-parameter name="ReceivingAddress" type="string" required=true %}
The address where you want to receive the exchanged amount.
{% endapi-method-parameter %}

{% api-method-parameter name="ReceivingAddressSignature" type="string" required=true %}
The signature of the receiving address. Must be the same format as X-REQUEST-SIGNATURE header.
{% endapi-method-parameter %}

{% api-method-parameter name="SourceAmount" type="string" required=true %}
The amount to be exchanged.
{% endapi-method-parameter %}

{% api-method-parameter name="Price" type="string" required=true %}
Price per source currency
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=201 %}
{% api-method-response-example-description %}
Newly created order.
{% endapi-method-response-example-description %}

```javascript
{
  "ID": "string (uuid)"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="RequestValidationWithBodyError" %}
```javascript
// Invalid request.
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="ApiKeySecretValidationError" %}
```javascript
// Invalid api key/secret format.
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="ValidationError" %}
```javascript
// Validation error. See inner errors for more details.
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="ApiKeySecretValidationError" %}
```javascript
// Unautorized api key/secret.
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="WebhookNotFoundError" %}
```javascript
// Forbidden.
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=503 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```javascript
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% code title="Request Example" %}
```javascript
{
  "Conversion": "string",
  "SendingAddress": "string",
  "SendingAddressSignature": "string",
  "ReceivingAddress": "string",
  "ReceivingAddressSignature": "string",
  "SourceAmount": "string",
  "Price": "string"
}
```
{% endcode %}

{% api-method method="patch" host="https://api.gluwa.com" path="/V1/Orders/:ID" %}
{% api-method-summary %}
Cancel Order
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="ID" type="string" required=true %}
Order ID
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Gluwa uses API keys and basic access authentication to authenticate this endpoint. Refer to "API Keys" in the Authentication section for details.
{% endapi-method-parameter %}

{% api-method-parameter name="Content-Types" type="string" required=true %}
application/json-patch+json, application/json, text/json, application/\*+json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="Action" type="string" required=true %}
Action to perform on the order. Only `"Cancel"` is available.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Order is canceled.
{% endapi-method-response-example-description %}

```javascript
{}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="RequestValidationWithBodyError" %}
```javascript
// Invalid request.
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="ValidationError" %}
```javascript
// Invalid api key/secret format.
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="ApiKeySecretValidationError" %}
```javascript
// Invalid api key/secret format.
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="ForbiddenRequestError" %}
```javascript
// Access to this order is denied.
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="ApiKeySecretValidationError" %}
```javascript
// Unautorized api key/secret.
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
The order is not found.
{% endapi-method-response-example-description %}

```javascript
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
The order cannot be canceled because it is already complete.
{% endapi-method-response-example-description %}

```javascript
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error.
{% endapi-method-response-example-description %}

```javascript
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% code title="Request Example" %}
```javascript
{
  "Action": "string"
}
```
{% endcode %}

{% api-method method="get" host="https://api.gluwa.com" path="/V1/Orders" %}
{% api-method-summary %}
Retrieve List of Orders
{% endapi-method-summary %}

{% api-method-description %}
Get a list of orders
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Gluwa uses API keys and basic access authentication to authenticate this endpoint. Refer to "API Keys" in the Authentication section for details.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="startDateTime" type="string" required=false %}
If defined, only orders created after this datetime are included in the response. Datetime must be in ISO 8601 format.
{% endapi-method-parameter %}

{% api-method-parameter name="endDateTime" type="string" required=false %}
If defined, only orders created before this datetime are included in the response. Datetime must be in ISO 8601 format.
{% endapi-method-parameter %}

{% api-method-parameter name="status" type="string" required=false %}
`enum ["Active", "Complete", "Canceled"]` Status of the order. Defaults to `"Active"`.
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="integer" required=false %}
`int64` Number of orders to skip the beginning of list. Used for pagination. Defaults to `0`.
{% endapi-method-parameter %}

{% api-method-parameter name="" type="string" required=false %}
`int32` Number of orders to include in the result. Defaults to `25`, maximum of `100`.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List of orders.
{% endapi-method-response-example-description %}

```javascript
[
  {
    "ID": "string (uuid)",
    "Conversion": "string",
    "SendingAddress": "string",
    "SourceAmount": "string",
    "Price": "string",
    "ReceivingAddress": "string",
    "Status": "string"
  }
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="RequestValidationWithoutBodyError" %}
```javascript
// Invalid request.
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="ApiKeySecretValidationError" %}
```javascript
// Invalid api key/secret format.
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Unauthorized api key/secret.
{% endapi-method-response-example-description %}

```javascript
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error.
{% endapi-method-response-example-description %}

```javascript
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="get" host="https://api.gluwa.com" path="/V1/Orders/:ID" %}
{% api-method-summary %}
Retrieve Order with ID
{% endapi-method-summary %}

{% api-method-description %}
Return an order with specified ID
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="ID" type="string" required=true %}
`uuid` The ID of an order
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Authorization" type="string" required=true %}
Gluwa uses API keys and basic access authentication to authenticate this endpoint. Refer to "API Keys" in the Authentication section for details.
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The order with a specified ID
{% endapi-method-response-example-description %}

```javascript
{
  "ID": "string (uuid)",
  "Conversion": "string",
  "SendingAddress": "string",
  "SourceAmount": "string",
  "Price": "string",
  "ReceivingAddress": "string",
  "Status": "string",
  "Exchanges": [
    {
      "SendingAddress": "string",
      "ReceivingAddress": "string",
      "SourceAmount": "string",
      "ExchangedAmount": "string",
      "Price": "string",
      "Status": "string"
    }
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="ApiKeySecretValidationError" %}
```javascript
// Invalid api key/secret format.
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="RequestValidationWithoutBodyError" %}
```javascript
// Invalid request.
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="ForbiddenRequestError" %}
```javascript
// Access to this order is denied.
{
  "InnerErrors": [
    {
      "Code": "string",
      "Path": "string",
      "Message": "string"
    }
  ],
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}

{% tab title="ApiKeySecretValidationError" %}
```javascript
// Unautorized api key/secret.
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Order is not found.
{% endapi-method-response-example-description %}

```javascript
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error.
{% endapi-method-response-example-description %}

```javascript
{
  "ID": "string (uuid)",
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

