---
description: Learn how to use various endpoints of the Gluwa API.
---

# API Reference

## Introduction

The Gluwa API follows [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) design guideline. The API has predictable resource-oriented URL's and returns [JSON-encoded](http://www.json.org/) responses, and uses standard HTTP response codes, authentication, and verbs.

We support a separate environment for testing purposes. When testing, use the sandbox credentials and URL. When you are ready to go live, use the live credentials and URL.

{% page-ref page="../development/environments.md" %}

#### Base URL

{% tabs %}
{% tab title="Live" %}
```http
https://api.gluwa.com
```
{% endtab %}

{% tab title="Sandbox" %}
```http
https://sandbox.api.gluwa.com
```
{% endtab %}
{% endtabs %}

#### Client SDK's

By default, the Gluwa API Docs demonstrate using curl to interact with the API over HTTP. Select one of our official Software Development Kits \(SDK\) to see examples in code.

## Authentication

The Gluwa API utilizes various authentication methods that fits each endpoint. Follow the link below to learn more.

{% page-ref page="authentication.md" %}

## Core Resources

{% api-method method="get" host="https://api.gluwa.com" path="/v1/:currency/Addresses/:address" %}
{% api-method-summary %}
Retrieve a Balance for an Address
{% endapi-method-summary %}

{% api-method-description %}
Get balance associated to an address.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="Currency" type="string" required=true %}
`"BTC"`, `"USDG"` or `"KRWG"`
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
	"Balance": string,
	"Currency": string,
	"Nonce": 0
}
```
{% endtab %}

{% tab title="includeUnspentOutputs=true" %}
```javascript
{
	"Balance": string,
	"Currency": string,
	"Nonce": 0,
	"UnspentOutputs": [
		{
		  "Amount": string
		  "TxHash": string
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

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=500 %}
{% api-method-response-example-description %}
Server error
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=503 %}
{% api-method-response-example-description %}
Service unavailable for specified currency
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% page-ref page="balance.md" %}

{% page-ref page="fee.md" %}

{% page-ref page="transaction.md" %}

{% page-ref page="qr-code.md" %}

{% page-ref page="quote.md" %}

{% page-ref page="order.md" %}

{% page-ref page="order-book.md" %}

