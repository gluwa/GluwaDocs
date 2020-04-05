# Order Book

## The Order Book List Item Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Amount** | `string` | Amount of available source currency. |
| **Price** | `string` | Price per source currency. |

{% code title="Example" %}
```javascript
{
  "Amount": "string",
  "Price": "string"
}
```
{% endcode %}



{% api-method method="get" host="https://api.gluwa.com" path="/V1/OrderBook/:conversion" %}
{% api-method-summary %}
Retrieve Order Book
{% endapi-method-summary %}

{% api-method-description %}
Get current order book.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="conversion" type="string" required=true %}
`enum ["UsdgKrwg", "KrwgUsdg"]` Conversion of the exchange. `"UsdgKrwg"` = USDG -&gt; KRWG, `"KrwgUsdg"` = KRWG -&gt; USDG
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-query-parameters %}
{% api-method-parameter name="limit" type="integer" required=false %}
`int32` Number of orders to include in the result. Ordered by descending price \(best price first\). Defaults to `100`, maximum of `1000`.
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List of active orders
{% endapi-method-response-example-description %}

```javascript
[
  {
    "Amount": "string",
    "Price": "string"
  }
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid request parameters
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

