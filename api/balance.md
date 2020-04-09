---
description: Learn about the Balance objects and its endpoints.
---

# Balance

This is an object representing the Bitcoin or Gluwacoin balance of a certain address. You can retrieve it to see the current balance on your Gluwa account.

You can also retrieve the nonce, which is used to sign the Ethereum contract.

## The Balance Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Balance** | `string` | A string representing how much balance in the currency unit \(e.g., `"1.23"` for 1.23 USD-G\). |
| **Currency** | `string` | `enum: ["BTC", "USDG", "KRWG", "NGNG"]` The currency unit of the balance: `"BTC"`, `"USDG"`,  `"KRWG"`, or `"NGNG"`. |
| **Nonce** | `long` | Used when signing Ethereum contract. Nonce is an integer that has to increase every time you make a transaction. For example, if you start from 0 for your very first transaction you make. The next transaction you make should be larger than 0. |

{% api-method method="get" host="https://api.gluwa.com" path="/v1/:currency/Addresses/:address" %}
{% api-method-summary %}
Retrieve Balance with Address
{% endapi-method-summary %}

{% api-method-description %}
Get balance associated to an address.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="Currency" type="string" required=true %}
`enum: ["BTC","USDG","KRWG","NGNG"]` BTC, USDG or KRWG
{% endapi-method-parameter %}

{% api-method-parameter name="Address" type="string" required=true %}
The address for which the balance will be retrieved
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="includeUnspentOutputs" type="boolean" required=false %}
Defaults to false. Unspent Transaction Outputs or UTXOs serve as globally-accessible evidence that you have Bitcoin in your digital wallet.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Balance and associated currency
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="includeUnspentOutputs=false" %}
```javascript
{
  "Balance": "string",
  "Currency": "string"
}
```
{% endtab %}

{% tab title="includeUnspentOutputs=true" %}
```javascript

{
  "Balance": "string",
  "Currency": "string",
	"UnspentOutputs": [
		{
		  "Amount": "string",
		  "TxHash": "string",
		  "Index": int
		  "Confirmations": int
		}
	]
}
```
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid address format
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
Service unavailable for specified currency
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

