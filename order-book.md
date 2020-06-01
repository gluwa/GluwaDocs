# Order Book

## `GET v1/OrderBook/:conversion`

Get current order book.

### Request

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| conversion | `string` | Conversion symbol. See [conversion](api/currency-and-conversion-symbols.md#conversion-symbols). |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | array of [Orders](order-book.md#order). |

#### Order

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Amount | `string` | Amount of available in the order. |
| Price | `string` | Price. The unit is `<exchanged currency>/<source currency>`. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |

