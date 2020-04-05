---
description: Learn about the Quote objects and its endpoints.
---

# Quote

You can retrieve a `Quote` object before committing for an exchange transaction. The object tells what price you can expect if you exchange your digital asset for another.

You can create, accept, and retrieve exchange quotes with Gluwa API.

## The Quote Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Conversion** | `string`  | Conversion for the exchange, from source currency to exchanged currency.  For example,`"UsdgKrwg"` for converting USD-G to KRW-G and `"KrwgUsdg"` for converting KRW-G to USDG. |
| **TotalSourceAmount** | `string` | Total amount of source currency that will be exchanged. |
| **TotalFee** | `string` | Total fee amount in source currency. |
| **TotalEstimatedExchangedAmount** | `string` | Total amount in exchanged currency that the user is estimated to receive. |
| **AveragePrice** | `string` | Price per source currency for the whole quote. |
| **BestPrice** | `string` | Price per source currency from the order that offered the best price. Currently, this is the same as AveragePrice field. |
| **WorstPrice** | `string` | Price per source currency from the order that offered the worst price. Currently, this is the same as AveragePrice field. |
| **MatchedOrders** | `array` | `array<MatchedOrder>` List of orders that are matched to the quote. |
| **CreatedDateTime** | `string` | Time when the quote is created. |
| **TimeToLive** | `integer` | Amount of time in seconds in which the quote is valid, from the time when the quote is created. |
| **Checksum** | `string` | The checksum for the quote. The user must include this Checksum when they accept the quote in their request. |

{% code title="Example" %}
```javascript
{
  "Conversion": "string",
  "TotalSourceAmount": "string",
  "TotalFee": "string",
  "TotalEstimatedExchangedAmount": "string",
  "AveragePrice": "string",
  "BestPrice": "string",
  "WorstPrice": "string",
  "MatchedOrders": [
    {
      "OrderID": "string (uuid)",
      "DestinationAddress": "string",
      "SourceAmount": "string",
      "Fee": "string",
      "ExchangedAmount": "string",
      "Price": "string",
      "Executor": "string"
    }
  ],
  "CreatedDateTime": "string (date-time)",
  "TimeToLive": "integer (int64)",
  "Checksum": "string"
}
```
{% endcode %}

## The Matched Order Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **OrderID** | `string` | ID of the order. |
| **DestinationAddress** | `string` | Address where the user's source currency will be sent to. |
| **SourceAmount** | `string` | Amount in source currency that this order will fulfill. |
| **Fee** | `string` | Fee amount in source currency. |
| **ExchangedAmount** | `string` | Amount in exchanged currency that this order will provide. |
| **Price** | `string` | Price per source currency. |
| **Executor** | `string` | Gluwa address that will execute the exchange on the user's behalf. You will need this to sign the reserve transaction signature when you accept the quote. |

{% code title="Example" %}
```javascript
{
  "OrderID": "string (uuid)",
  "DestinationAddress": "string",
  "SourceAmount": "string",
  "Fee": "string",
  "ExchangedAmount": "string",
  "Price": "string",
  "Executor": "string"
}
```
{% endcode %}

## The Accept Quote Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Conversion** | `string` | Conversion for the exchange, from source currency to exchanged currency. `"UsdgKrwg"` for converting USD-G to KRW-G and `"KrwgUsdg"` for converting KRW-G to USDG. |
| **MatchedOrders** | `array` | `array<AcceptQuoteMatchedOrder>` List of orders that are matched to the quote. |
| **CreatedDateTime** | `string` | Time when the quote is created. This is provided by the API when the quote is created. |
| **TimeToLive** | `integer` | Amount of time in seconds in which the quote is valid, from the time when the quote is created. This is provided by the API when the quote is created. |
| **Checksum** | `string` | The checksum for the quote. The user must include this Checksum when they accept the quote in their request. |
| **ReceivingAddress** | `string` | The user's address that will receive the exchanged currency. |
| **ReceivingAddressSignature** | `string` | Signature of the receiving address. Must be the same format as X-REQUEST-SIGNATURE header. |

{% code title="Example" %}
```javascript
{
  "Conversion": "string",
  "MatchedOrders": [
    {
      "OrderID": "string (uuid)",
      "SendingAddress": "string",
      "DestinationAddress": "string",
      "SourceAmount": "string",
      "ExchangedAmount": "string",
      "Price": "string",
      "Fee": "string",
      "Nonce": "string",
      "ExpiryBlockNumber": "string",
      "Executor": "string",
      "Signature": "string"
    }
  ],
  "CreatedDateTime": "string (date-time)",
  "TimeToLive": "integer (int64)",
  "Checksum": "string",
  "ReceivingAddress": "string",
  "ReceivingAddressSignature": "string"
}
```
{% endcode %}

## The Accept Quote Matched Order object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **OrderID** | `string` | ID of the order that was matched. |
| **SendingAddress** | `string` | Address that the user's source currency will be sent from. |
| **DestinationAddress** | `string` | Address that the user's source currency will be sent to. |
| **SourceAmount** | `string` | Amount in source currency that the user will send for this order. |
| **ExchangedAmount** | `string` | Amount in exchanged currency that the user will receive from this order. |
| **Price** | `string` | Price per source currency. |
| **Fee** | `string` | Fee amount in source currency. |
| **Nonce** | `string` | Nonce for the reserve function. |
| **ExpiryBlockNumber** | `string` | Block number at which the reserve function will expire. |
| **Executor** | `string` | Gluwa address that will execute the exchange on the user's behalf. |
| **Signature** | `string` | Signature of the reserve function. |

{% code title="Example" %}
```javascript
{
  "OrderID": "string (uuid)",
  "SendingAddress": "string",
  "DestinationAddress": "string",
  "SourceAmount": "string",
  "ExchangedAmount": "string",
  "Price": "string",
  "Fee": "string",
  "Nonce": "string",
  "ExpiryBlockNumber": "string",
  "Executor": "string",
  "Signature": "string"
}
```
{% endcode %}

{% api-method method="post" host="https://api.gluwa.com" path="/v1/Quote" %}
{% api-method-summary %}
Create Pending Quote
{% endapi-method-summary %}

{% api-method-description %}
Create a new quote for your anticipated exchange.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-Types" type="string" required=true %}
application/json-patch+json, application/json, application/\*+json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="Amount" type="string" required=true %}
Amount in source currency.
{% endapi-method-parameter %}

{% api-method-parameter name="Conversion" type="string" required=true %}
Conversion for the exchange, from source currency to exchanged currency. `"UsdgKrwg"` for converting USD-G to KRW-G and `"KrwgUsdg"` for converting KRW-G to USDG.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Newly generated quote
{% endapi-method-response-example-description %}

```javascript
{
  "Conversion": "string",
  "TotalSourceAmount": "string",
  "TotalFee": "string",
  "TotalEstimatedExchangedAmount": "string",
  "AveragePrice": "string",
  "BestPrice": "string",
  "WorstPrice": "string",
  "MatchedOrders": [
    {
      "OrderID": "string (uuid)",
      "DestinationAddress": "string",
      "SourceAmount": "string",
      "Fee": "string",
      "ExchangedAmount": "string",
      "Price": "string",
      "Executor": "string"
    }
  ],
  "CreatedDateTime": "string (date-time)",
  "TimeToLive": "integer (int64)",
  "Checksum": "string"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="RequestValidationWithBodyError" %}
```javascript
// Invalid request body
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

{% tab title="ValidationError\[ECreateQuoteError\]" %}
```javascript
// Validation error. See inner errors for more details
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

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
No matching orders available
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
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% code title="Request Example" %}
```javascript
{
  "Amount": "string",
  "Conversion": "string"
}
```
{% endcode %}

{% api-method method="put" host="https://api.gluwa.com" path="/v1/Quote" %}
{% api-method-summary %}
Accept Pending Quote
{% endapi-method-summary %}

{% api-method-description %}
Accept the pending quote that has been provided by the GetPendingQuote endpoint.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-Types" type="string" required=true %}
application/json-patch+json, application/json, application/\*+json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="Conversion" type="string" required=true %}
Conversion for the exchange, from source currency to exchanged currency. `"UsdgKrwg"` for converting USD-G to KRW-G and `"KrwgUsdg"` for converting KRW-G to USDG.
{% endapi-method-parameter %}

{% api-method-parameter name="MatchedOrders" type="array" required=true %}
List of orders that are matched to the quote. See Accept Quote Matched Order object.
{% endapi-method-parameter %}

{% api-method-parameter name="CreatedDateTime" type="string" required=true %}
Time when the quote is created. This is provided by the API when the quote is created.
{% endapi-method-parameter %}

{% api-method-parameter name="TimeToLive" type="integer" required=true %}
Amount of time in seconds in which the quote is valid, from the time when the quote is created. This is provided by the API when the quote is created.
{% endapi-method-parameter %}

{% api-method-parameter name="ReceivingAddress" type="string" required=true %}
The user's address that will receive the exchanged currency.
{% endapi-method-parameter %}

{% api-method-parameter name="ReceivingAddressSignature" type="string" required=true %}
Signature of the receiving address. Must be the same format as X-REQUEST-SIGNATURE header.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=202 %}
{% api-method-response-example-description %}
Quote is accepted
{% endapi-method-response-example-description %}

```javascript
{
  "ID": "string (uuid)"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid request
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="RequestValidationWithBodyError" %}
```javascript
// Invalid request body.
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

{% tab title="ValidationError\[EAcceptQuoteError\]" %}
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

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Order is not found or not available.
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
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% code title="Request Example" %}
```javascript
{
  "Conversion": "string",
  "MatchedOrders": [
    {
      "OrderID": "string (uuid)",
      "SendingAddress": "string",
      "DestinationAddress": "string",
      "SourceAmount": "string",
      "ExchangedAmount": "string",
      "Price": "string",
      "Fee": "string",
      "Nonce": "string",
      "ExpiryBlockNumber": "string",
      "Executor": "string",
      "Signature": "string"
    }
  ],
  "CreatedDateTime": "string (date-time)",
  "TimeToLive": "integer (int64)",
  "Checksum": "string",
  "ReceivingAddress": "string",
  "ReceivingAddressSignature": "string"
}
```
{% endcode %}

{% api-method method="get" host="https://api.gluwa.com" path="/V1/:currency/Addresses/:address/Quotes" %}
{% api-method-summary %}
Retrieve List of Quotes
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="currency" type="string" required=true %}
Currency type. \[USDG, KRWG\]
{% endapi-method-parameter %}

{% api-method-parameter name="address" type="string" required=true %}
User's public address.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Content-Types" type="string" required=true %}
application/json-patch+json, application/json, text/json, application/\*+json
{% endapi-method-parameter %}

{% api-method-parameter name="X-REQUEST-SIGNATURE" type="string" required=true %}
Base64 encoding string of {unixtimestamp}.{signature}
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="startDateTime" type="string" required=false %}
If defined, only quotes created after this datetime are included in the response. Datetime must be in ISO 8601 format.
{% endapi-method-parameter %}

{% api-method-parameter name="endDateTime" type="string" required=false %}
If defined, only quotes created before this datetime are included in the response. Datetime must be in ISO 8601 format.
{% endapi-method-parameter %}

{% api-method-parameter name="status" type="string" required=false %}
Status of the quote. Defaults to Pending. \[Pending, Processed\]
{% endapi-method-parameter %}

{% api-method-parameter name="offset" type="integer" required=false %}
Number of quotes to skip the beginning of list. Used for pagination. Defaults to 0.
{% endapi-method-parameter %}

{% api-method-parameter name="limit" type="integer" required=false %}
Number of quotes to include in the result. Defaults to 25, maximum of 100.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Success
{% endapi-method-response-example-description %}

```javascript
[
  {
    "ID": "string (uuid)",
    "SendingAddress": "string",
    "SourceAmount": "string",
    "Fee": "string",
    "EstimatedExchangedAmount": "string",
    "WeightedAveragePrice": "string",
    "BestPrice": "string",
    "WorstPrice": "string",
    "ReceivingAddress": "string",
    "Status": "string",
    "Conversion": "string"
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
// Request validation without body error. See inner errors for more details.
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

{% tab title="AddressSignatureValidationError" %}
```javascript
// Address signature validation error. See inner errors for more details.
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

{% api-method method="get" host="https://api.gluwa.com" path="/V1/Quotes/:ID" %}
{% api-method-summary %}
Retrieve Quote with ID
{% endapi-method-summary %}

{% api-method-description %}
Get a quote with specified ID
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="ID" type="string" required=true %}
The ID of a quote.
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="Content-Types" type="string" required=true %}
application/json-patch+json, application/json, text/json, application/\*+json
{% endapi-method-parameter %}

{% api-method-parameter name="X-REQUEST-SIGNATURE" type="string" required=true %}
Base64 encoding string of {unixtimestamp}.{signature}
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
The quote with a specified ID.
{% endapi-method-response-example-description %}

```javascript
{
  "SendingAddress": "string",
  "SourceAmount": "string",
  "Fee": "string",
  "EstimatedExchangedAmount": "string",
  "ReceivingAddress": "string",
  "Status": "string",
  "Conversion": "string",
  "MatchedOrders": [
    {
      "SourceAmount": "string",
      "Fee": "string",
      "Status": "string",
      "ExchangedAmount": "string",
      "Price": "string"
    }
  ]
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid request.
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

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Access to this quote is denied.
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

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Quote is not found.
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
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

