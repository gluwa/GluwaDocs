---
description: Learn about the Quote objects and its endpoints.
---

# Quote

You can retrieve a `Quote` object before committing for an exchange transaction. The object tells what price you can expect if you exchange your digital asset for another.

You can create, accept, and retrieve exchange quotes with Gluwa API.

## `POST /v1/Quote`

Get Quote for currency exchange

### Request

#### Request Body

<table>
  <thead>
    <tr>
      <th style="text-align:left">Attribute</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Amount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The amount in source currency you want to exchange.</td>
    </tr>
    <tr>
      <td style="text-align:left">Conversion</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Conversion symbol for the exchange. See <a href="all-supported-currencies.md#conversion-symbols">conversion</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left">SendingAddress</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The address that will fund the source amount.</td>
    </tr>
    <tr>
      <td style="text-align:left">SendingAddressSignature</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The signature of the sending address. Generated the same way as <a href="authentication.md#x-request-signature">X-REQUEST-SIGNATURE</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left">ReceivingAddress</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>The address that the exchanged currency will be received.</p>
        <p></p>
        <p>For example, if the conversion is <code>BtcUsdg</code>, your receiving
          address must be a <code>USD-G</code> address since your exchanged funds will
          be sent to that address.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ReceivingAddressSignature</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The signature of the receiving address. Generated the same way as <a href="authentication.md#x-request-signature">X-REQUEST-SIGNATURE</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left">BtcPublicKey</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em> Required if the source currency is <code>BTC</code>.
        This is BTC public key of the sending address that will be used to reserve
        your funds.</td>
    </tr>
  </tbody>
</table>### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | [Quote](quote.md#quote). |

#### Quote

| Attribute | Type | Description |
| :--- | :--- | :--- |
| Conversion | `string` | The [conversion](all-supported-currencies.md#conversion-symbols) of the exchange. |
| TotalSourceAmount | `string` | The total amount in source currency that will be exchanged. |
| TotalFee | `string` | The exchange fee. |
| TotalEstimatedExchangedAmount | `string` | The total estimated exchanged amount in exchanged currency. |
| AveragePrice | `string` | The average of all the prices in the list of matched orders. The unit is `<exchanged currency>/<source currency>`. |
| BestPrice | `string` | The best price available in the list of matched orders. The unit is `<exchanged currency>/<source currency>`. |
| WorstPrice | `string` | The best price available in the list of matched orders. The unit is `<exchanged currency>/<source currency>`. |
| MatchedOrders | `array of` [`MatchedOrder`](quote.md#matchedorder)\`\` | The list of matched orders available to fulfill the requested source amount.  |
| CreatedDateTime | `DateTime` | The time when the quote is created. |
| TimeToLive | `int` | The duration in seconds that the quote is valid. |
| Checksum | `string` | Checksum. Used when you accept the quote. |

#### MatchedOrder

<table>
  <thead>
    <tr>
      <th style="text-align:left">Attribute</th>
      <th style="text-align:left">Type</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">OrderID</td>
      <td style="text-align:left"><code>UUID</code>
      </td>
      <td style="text-align:left">Order ID</td>
    </tr>
    <tr>
      <td style="text-align:left">DestinationAddress</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The address where the source amount must be sent to.</td>
    </tr>
    <tr>
      <td style="text-align:left">SourceAmount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The amount in source currency that this order will exchange.</td>
    </tr>
    <tr>
      <td style="text-align:left">Fee</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The fee amount that must be used to create <code>ReserveTxnSignature</code>, <code>ExecuteTxnSignature</code> and <code>ReclaimTxnSignature</code>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ExchangedAmount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The amount in exchanged currency that this order will fulfill.</td>
    </tr>
    <tr>
      <td style="text-align:left">Price</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The price this order is offering for the exchange. The unit is <code>&lt;exchanged currency&gt;/&lt;source currency&gt;</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">ExpiryBlockNumber</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p><em><b>Optional. </b></em>Required if the source currency is Gluwacoin
          currency (ex&gt; <code>USD-G</code>, <code>KRW-G</code>). The block number
          where your reserved funds will expire. You need this to create the reserve
          transaction signature.</p>
        <p></p>
        <p>After this block number, the exchange will not execute, and you may call
          reclaim function on the blockchain to release your reserved funds.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Executor</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em> Required if the source currency is Gluwacoin
        currency (ex&gt; <code>USD-G</code>, <code>KRW-G</code>). The address that
        will execute the exchange on your behalf. You need this to sign the reserve
        transaction signature.</td>
    </tr>
    <tr>
      <td style="text-align:left">ReservedFundsAddress</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em> Required if the source currency is <code>BTC</code>.
        The address where the source amount must be sent to to reserve your funds
        for the exchange.</td>
    </tr>
    <tr>
      <td style="text-align:left">ReservedFundsRedeemScript</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em> Required if the source currency is <code>BTC</code>.</td>
    </tr>
  </tbody>
</table>### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `MissingBody` | Request body is missing. |
| 400 | `InvalidBody` | Request validation errors. See InnerErrors. |
| 400 | `ValidationError` | Request validation errors. See InnerErrors. |
| 404 | `NotFound` | No orders are matched. |
| 500 | `InternalServerError` | Server error. |

## `PUT /v1/Quote`

Accept quote received from `POST /v1/Quote` endpoint.

### Request

#### Request Body

| Attribute | Type | Description |
| :--- | :--- | :--- |
| MatchedOrders | array of [MatchedOrder](quote.md#matchedorder-1) | All orders that will fulfill this quote. |
| Checksum | `string` | Checksum that was received when quote was created. |

#### MatchedOrder

| Attribute | Type | Description |
| :--- | :--- | :--- |
| OrderID | `UUID` | ID of the order that was matched |
| ReserveTxnSignature | `string` | Reserve transaction signature used to reserve funds for the exchange. |
| Nonce | `string` | _**Optional**_. Required if the source currency is Gluwacoin currency \(ex&gt; `USD-G`, `KRW-G`\). Nonce is an integer used when creating reserve transaction signature. It must increase each time you make any new transactions \(transfer, exchange, etc\). |
| ExecuteTxnSignature | `string` | _**Optional.**_ Required if the source currency `BTC`. Execute transaction signature used to execute the exchange when your funds are available in the reserve address. |
| ReclaimTxnSignature | `string` | _**Optional.**_ Required if the source currency `BTC`. Reclaim transaction signature used to return the funds in the reserve address when the exchange fails or expires. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 202 | [AcceptedQuoteID](quote.md#acceptedquoteid). |

#### AcceptedQuoteID

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `UUID` | The ID of the accepted quote. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 400 | `MissingBody` | Request body is missing. |
| 400 | `InvalidBody` | Request validation errors. See InnerErrors. |
| 400 | `ValidationError` | Request validation errors. See InnerErrors. |
| 403 | `Forbidden` | Invalid checksum. Checksum may be wrong or expired. |
| 404 | `NotFound` | One of the matched orders are no longer available. |
| 500 | `InternalServerError` | Server error. |

## `GET /v1/:currency/Addresses/:address/Quotes`

Get a list of accepted quotes.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| X-REQUEST-SIGNATURE | `string` | Address Signature of the sending address. See [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| currency | `string` | The source [currency](all-supported-currencies.md#conversion-symbols) of the quote. |
| address | `string` | The sending address of the quote. |

#### Query Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| startDateTime | `datetime` | ISO 8601 format datetime. If defined, only quotes created after this datetime are included in the response. |
| endDateTime | `datetime` | ISO 8601 format datetime. If defined, only quotes created before this datetime are included in the response. |
| status | `string` | `Pending` or `Processed`. |
| offset | `int` | Number of quotes to skip the beginning of list. Defaults to 0. |
| limit | `int` | Number of quotes to include in the result. Defaults to 25, maximum of 100. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | array of [Quotes](quote.md#quote-1). |

#### Quote

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `UUID` | Accepted quote ID. |
| SendingAddress | `string` | The address that funded the source amount. |
| SourceAmount | `string` | The total amount  |
| Fee | `string` | The total fee of the exchange |
| EstimatedExchangedAmount | `string` | The estimated exchange amount. Sometimes, if someone takes the order before you, the exchange will not be executed. |
| AveragePrice | `string` | The average of all the prices in the list of matched orders. The unit is `<exchanged currency>/<source currency>`. |
| BestPrice | `string` | The best price available in the list of matched orders. The unit is `<exchanged currency>/<source currency>`. |
| WorstPrice | `string` | The best price available in the list of matched orders. The unit is `<exchanged currency>/<source currency>`. |
| ReceivingAddress | `string` | The address that the exchanged currency is received. |
| Status | `string` | `Pending` or `Processed`. |
| Conversion | `string` | The conversion of the quote. See [conversion](all-supported-currencies.md#conversion-symbols). |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 403 | `SignatureMissing` | `X-REQUEST-SIGNATURE` header is missing. |
| 403 | `SignatureExpired` | `X-REQUEST-SIGNATURE` has expired. |
| 403 | `InvalidSignature` | Invalid `X-REQUEST-SIGNATURE`. |
| 500 | `InternalServerError` | Server error. |

## `GET /V1/Quotes/:ID`

Get an accepted quote with ID.

### Request

#### Headers

| Header | Type | Description |
| :--- | :--- | :--- |
| X-REQUEST-SIGNATURE | `string` | Address Signature of the sending address of this quote. See [X-REQUEST-SIGNATURE](authentication.md#x-request-signature). |

#### Path Parameters

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `UUID` | ID of the accepted quote. |

### Response

| HTTP Status | Return Object |
| :--- | :--- |
| 200 | [Quote](quote.md#quote-1). |

#### Quote

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `UUID` | Accepted quote ID. |
| SendingAddress | `string` | The address that funded the source amount. |
| SourceAmount | `string` | The total source amount. |
| Fee | `string` | The total fee of the exchange. |
| EstimatedExchangedAmount | `string` | The estimated exchange amount. Sometimes, if someone takes the order before you, the exchange will not be executed. |
| ReceivingAddress | `string` | The address that the exchanged currency is received. |
| Status | `string` | `Pending` or `Processed`. |
| Conversion | `string` | The conversion of the quote. See [conversion](all-supported-currencies.md#conversion-symbols). |
| MatchedOrders | `array of` [`MatchedOrders`](quote.md#matchedorder-2)\`\` | The list of matched orders that fulfilled the source amount.  |

#### MatchedOrder

| Attribute | Type | Description |
| :--- | :--- | :--- |
| SourceAmount | `string` | The amount in source currency that this order will exchange. |
| Fee | `string` | The fee amount used to create `ReserveTxnSignature`, `ExecuteTxnSignature` and `ReclaimTxnSignature` |
| Status | `string` | `Pending`, `Success` or `Failed`. |
| Price | `string` | The price this order is offering for the exchange. The unit is `<exchanged currency>/<source currency>`. |

### Errors

| HTTP Status | Error Code | Description |
| :--- | :--- | :--- |
| 400 | `InvalidUrlParameters` | Invalid URL parameters |
| 403 | `SignatureMissing` | `X-REQUEST-SIGNATURE` header is missing. |
| 403 | `SignatureExpired` | `X-REQUEST-SIGNATURE` has expired. |
| 403 | `InvalidSignature` | Invalid `X-REQUEST-SIGNATURE`. |
| 404 | `NotFound` | Quote not found. |
| 500 | `InternalServerError` | Server error. |

