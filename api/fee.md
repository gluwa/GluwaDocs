---
description: Learn about the Fee objects and its endpoints.
---

# Fee

To learn the recommended fee amount for a transaction, you can retrieve a `Fee` object. A blockchain charges transaction fees for processing  transaction \(e.g., [Bitcoin transaction fee](https://en.wikipedia.org/wiki/Bitcoin#Transaction_fees)\). Though transaction fees are optional, a transaction with higher fee gets processed first. Fee object includes the recommended minimum transaction fee, which is expected to get your transaction processed under a reasonable amount of time.

## The Fee Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Currency** | `string` | The currency unit of the fee |
| **MinimumFee** | `string` | The minimum recommended transaction fee  for the currency |

{% api-method method="get" host="https://api.gluwa.com" path="/v1/:currency/Fee" %}
{% api-method-summary %}
Retrieve Transaction Fee Recommendation
{% endapi-method-summary %}

{% api-method-description %}
Get recommended minimum fee for Bitcoin or Gluwacoin. The fee is dynamically calculated based on the current fee market.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="Currency" type="string" required=true %}
`enum: ["BTC","USDG","KRWG","NGNG"]` Currency of the query
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Fee response
{% endapi-method-response-example-description %}

```javascript
{
  "Currency": "string",
  "MinimumFee": "string"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid request
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
Server error
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
Service unavailable for this currency
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

### Transaction

