# Transaction

To transfer funds to another user, you can create a `Transaction` object. You can retrieve an individual transaction as well as list all transactions. Transactions are identified by a unique, random `TxnHash`.

## The Transaction Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Status** | `string` | The status of the transaction:`"Unconfirmed"`, `"Confirmed"`, or `"Failed"` |
| **Amount** | `string` | The amount of the transaction. If you are the sender, this is the amount deducted from your account. If you are the receiver, this is the amount added to your account. For the Transaction History endpoint, this can be a positive or negative value depending on whether it was added to or deducted from your account. |
| **Fee** | `string` | _optional_ Transaction fee. |
| **TotalAmount** | `string` | The sum of the `Amount` and the `Fee`. |
| **Currency** | `string` | The currency unit of the transaction |
| **Sources** | `string[]` | The source of the transaction. |
| **Targets** | `string[]` | The target of the transaction. |
| **TxnHash** | `string` | Unique identifier of the transaction |
| **CreatedDateTime** | `datetime` | Time at which the object was created. |
| **ModifiedDateTime** | `datetime` | Time at which the object was last modified. |
| **MerchantOrderID** | `string` | _optional_  ****Used by the receiver to identify a payment. Supported by QR code payment feature. |
| **Note** | `string` | _optional_  Whatever message that the sender wanted to put. It is an optional memo you can associate with the transaction. |

{% code title="Example" %}
```javascript
{
  "TxnHash": "string",
  "Amount": "string",
  "Fee": "string",
  "TotalAmount": "string",
  "Currency": "string",
  "Sources": [
    "string"
  ],
  "Targets": [
    "string"
  ],
  "MerchantOrderID": "string",
  "Note": "string",
  "Status": "string",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)"
}
```
{% endcode %}

## Transaction Status

| Status | Description |
| :--- | :--- |
| **Unconfirmed** | The transaction was announce to the blockchain, but is not included in any block yet. |
| **Confirmed** | The transaction was included in the blockchain and received a confirmation. |
| **Failed** | The transaction has failed for some reason. |

{% api-method method="get" host="https://api.gluwa.com" path="/v1/:currency/Addresses/:address/Transactions" %}
{% api-method-summary %}
List Transaction History for an Address
{% endapi-method-summary %}

{% api-method-description %}
Get transaction history for a given address.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="Currency" type="string" required=true %}
`enum: ["BTC","USDG","KRWG","NGNG"]` currency type
{% endapi-method-parameter %}

{% api-method-parameter name="Address" type="string" required=true %}
Cryptocurrency wallet address
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="X-REQUEST-SIGNATURE" type="string" required=true %}
Base64 encoding string of `{unixtimestamp}.{signature}`
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="Limit" type="integer" required=false %}
Amount of entries displayed \(max 100\)
{% endapi-method-parameter %}

{% api-method-parameter name="Status" type="string" required=false %}
**Confirmed** \(default\) - includes confirmed transactions on the blockchain, with additional data from Gluwa if they exist  
**Incomplete** - only includes unconfirmed and failed transactions in Gluwa  
{% endapi-method-parameter %}

{% api-method-parameter name="Offset" type="integer" required=false %}
Select which entry should the response start from
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
List of transactions associated with the address
{% endapi-method-response-example-description %}

```javascript
[
  {
    "TxnHash": "string",
    "Amount": "string",
    "Fee": "string",
    "TotalAmount": "string",
    "Currency": "string",
    "Sources": [
      "string"
    ],
    "Targets": [
      "string"
    ],
    "MerchantOrderID": "string",
    "Note": "string",
    "Status": "string",
    "CreatedDateTime": "string (date-time)",
    "ModifiedDateTime": "string (date-time)"
  }
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid request. For example, the address is in an invalid format.
{% endapi-method-response-example-description %}

{% tabs %}
{% tab title="RequestValidationWithoutBodyError" %}
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
{% endtab %}

{% tab title="BadRequestError" %}
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
{% endtab %}
{% endtabs %}
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Request signature header is not valid
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

{% api-method-response-example httpCode=503 %}
{% api-method-response-example-description %}
Service unavailable
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

{% api-method method="get" host="https://api.gluwa.com" path="/v1/:currency/Transactions/:txnhash" %}
{% api-method-summary %}
Retrieve Transaction Details by Hash
{% endapi-method-summary %}

{% api-method-description %}
Get transaction details for a given transaction hash.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="TxnHash" type="string" required=true %}
The hash associated to the transaction
{% endapi-method-parameter %}

{% api-method-parameter name="Currency" type="string" required=true %}
`enum: ["BTC","USDG","KRWG","NGNG"]` currency type
{% endapi-method-parameter %}
{% endapi-method-path-parameters %}

{% api-method-headers %}
{% api-method-parameter name="x-request-signature" type="string" required=true %}
Base64 encoding string of `{unixtimestamp}.{signature}`
{% endapi-method-parameter %}
{% endapi-method-headers %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Transaction response
{% endapi-method-response-example-description %}

```javascript
{
  "TxnHash": "string",
  "Amount": "string",
  "Fee": "string",
  "TotalAmount": "string",
  "Currency": "string",
  "Sources": [
    "string"
  ],
  "Targets": [
    "string"
  ],
  "MerchantOrderID": "string",
  "Note": "string",
  "Status": "string",
  "CreatedDateTime": "string (date-time)",
  "ModifiedDateTime": "string (date-time)"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid transaction hash format
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
Request signature header is not valid.
{% endapi-method-response-example-description %}

```javascript
{
  "Code": "string",
  "Message": "string",
  "ExtraData": "string"
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Transaction not found
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

{% api-method-response-example httpCode=503 %}
{% api-method-response-example-description %}
Service unavailable
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

{% api-method method="post" host="https://api.gluwa.com" path="/v1/Transactions" %}
{% api-method-summary %}
Create New Transaction
{% endapi-method-summary %}

{% api-method-description %}
Create a new transaction  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-Type" type="string" required=false %}
application/json-patch+json, application/json, text/json, application/\*+json
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-body-parameters %}
{% api-method-parameter name="Signature" type="string" required=true %}
A signature of the request body signed with the sender's private key. See "**How to Create a Signature for Create Transaction Endpoint**" below for details.
{% endapi-method-parameter %}

{% api-method-parameter name="Amount" type="string" required=true %}
Transaction amount, not including the fee. This is the amount that the receiver receives. If you want to send 1.85 BTC, put `"1.85"` here.
{% endapi-method-parameter %}

{% api-method-parameter name="Fee" type="string" required=true %}
Transaction fee amount. Note that there is a recommended transaction fee amount and you can get it from **Retrieve a Fee for a Transaction** endpoint. Your transaction may not get processed by Gluwa if the fee amount is smaller than the recommended amount.
{% endapi-method-parameter %}

{% api-method-parameter name="Currency" type="string" required=true %}
`enum: ["BTC","USDG","KRWG","NGNG"]` currency type
{% endapi-method-parameter %}

{% api-method-parameter name="Source" type="string" required=true %}
Address of the sender
{% endapi-method-parameter %}

{% api-method-parameter name="Target" type="string" required=true %}
Address of the receiver
{% endapi-method-parameter %}

{% api-method-parameter name="MerchantOrderID" type="string" required=false %}
`range: (up to 60 chars)` A user-determined value which is used to identify the purpose of the transaction. Supported by QR code payment feature.
{% endapi-method-parameter %}

{% api-method-parameter name="Note" type="string" required=false %}
`range: (up to 280 chars)` Whatever message that the sender wanted to put. It is an optional memo you can associate with the transaction.
{% endapi-method-parameter %}

{% api-method-parameter name="Nonce" type="string" required=false %}
Used when signing Ethereum contract. Nonce is an integer that has to increase every time you make a transaction. For example, if you start from 0 for your very first transaction you make. The next transaction you make should be larger than 0. You can find your current nonce when you call **Retrieve Balance for an Address** endpoint for Gluwacoin addresses.
{% endapi-method-parameter %}

{% api-method-parameter name="Idem" type="string" required=false %}
`uuid` Used to prevent submitting duplicate requests. For example, when you submit a request and the server accepted the request but for some reason the server returns an error, you will retry with the exactly the same request. The server will fail this duplicated request.
{% endapi-method-parameter %}

{% api-method-parameter name="PaymentID" type="string" required=false %}
`uuid` A unique identifier for a payment. Supported by QR code payment feature.
{% endapi-method-parameter %}

{% api-method-parameter name="PaymentSig" type="string" required=false %}
Required if `PaymentID` is not null. Must be verified when submitted with `PaymentID`
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=202 %}
{% api-method-response-example-description %}
The request has been accepted for processing, but the processing does not get completed immediately. Gluwa does not return the transaction hash immediately since it is not determined until we send the transaction information to the blockchain.
{% endapi-method-response-example-description %}

```javascript
{
  "TxnHash": "string"
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

{% tab title="ValidationError\[ECreateTransactionValidationError\]" %}
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

{% tab title="BtcSignatureValidationError" %}
```javascript
// (BTC only) Signed BTC transaction could not be verified.
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
For payments, payment signature could not be verified.
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
A transaction with the same transaction hash, payment ID, or idem already exists.
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
Service unavailable
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

## ETHless Transfer

Gluwacoin is an [ERC-20](https://en.wikipedia.org/wiki/ERC-20) token. A user can transfer Gluwacoin as he or she would transfer any other ERC-20 token, but the user will need to spend Ether to do so. Gluwa allows its users to move Gluwacoin without Ether by paying the transaction fee in Gluwacoin to Gluwa instead. We call this _ETHless transfer_.

#### How to Create a Signature for Create Transaction Endpoint

{% code title="Creating Signature for Create Transaction" %}
```javascript
var ethers = require('ethers');

// create an array of message values
const messageValues = [
    '{Contract Address}',// choose the contract of the token you are using
    '{Source}',
    '{Target}',
    // Note that Amount and Fee is in uint256, not int.
    // You can convert int to uint256 by multiplying the int value with 10^18.
    ethers.utils.parseEther({Amount in uint256}).toString(),// Ethereum uses uint256
    ethers.utils.parseEther({Fee in uint256}).toString(),// Ethereum uses uint256
    String({Nonce}),
];

const messageHash = ethers.utils.solidityKeccak256(
    [ 'address', 'address', 'address', 'uint256', 'uint256', 'uint256' ],
    messageValues
);

const messageHashBinary = ethers.utils.arrayify(messageHash);

let wallet = new ethers.Wallet('{Sender\'s private key}');

// creates a Promise with the Signature
let signPromise = wallet.signMessage(messageHashBinary);
```
{% endcode %}

{% hint style="info" %}
Note that the code for generating signature for USD-G or KRW-G requires the amount and fee in `uint256, but the rquest body requires the amount and fee int`. Multiply 10^18 to the `int` value to convert it to `uint`. For example, 1 in `int` is 1000000000000000000 in `uint256`.
{% endhint %}

## Gluwacoin Contracts

* USD Gluwacoin - [0xfb0aaa0432112779d9ac483d9d5e3961ece18eec](https://etherscan.io/token/0xfb0aaa0432112779d9ac483d9d5e3961ece18eec)
* KRW Gluwacoin - [0x4cc8486f2f3dce2d3b5e27057cf565e16906d12d](https://etherscan.io/token/0x4cc8486f2f3dce2d3b5e27057cf565e16906d12d)
* NGN Gluwacoin - [0x4AB30B965A8Ef0F512DA064B5e574d9Ad73c0e79](https://etherscan.io/token/0x4AB30B965A8Ef0F512DA064B5e574d9Ad73c0e79)

