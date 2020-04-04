# API Reference

## Introduction

The Gluwa API follows [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) design guideline. The API has predictable resource-oriented URL's and returns [JSON-encoded](http://www.json.org/) responses, and uses standard HTTP response codes, authentication, and verbs.

We support a separate environment for testing purposes. When testing, use the sandbox credentials and URL. When you are ready to go live, use the live credentials and URL.

{% page-ref page="../development/environments.md" %}

{% tabs %}
{% tab title="Live" %}
{% code title="Base URL" %}
```http
https://api.gluwa.com
```
{% endcode %}
{% endtab %}

{% tab title="Sandbox" %}
{% code title="Base URL" %}
```http
https://sandbox.api.gluwa.com
```
{% endcode %}
{% endtab %}
{% endtabs %}

## Authentication

### HTTPS

All API requests must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTP_Secure). Calls made over plain HTTP will fail. API requests without proper authentication will also fail.

### Signature

Signature is used to verify the ownership of a cryptocurrency \(e.g., [Bitcoin](https://en.wikipedia.org/wiki/Bitcoin) or [Ethereum](https://en.wikipedia.org/wiki/Ethereum)\) wallet. A cryptocurrency wallet is a set of a private key and a public key. You can verify the ownership of a cryptocurrency wallet by encrypting an arbitrary message with its private key. Anyone who has the public key can decrypt the encryption with it and get the original text.

#### Gluwacoin Signature

To verify the ownership of a Gluwacoin wallet, you need to create an Ethereum signature. This is because Gluwacoins are [ERC-20](https://en.wikipedia.org/wiki/ERC-20) tokens. If you create a signature of the transfer information, Gluwacoin smart contract can use it as a proof-of-intention.

Think of signature as a signed contract. You send the contract to Gluwa to submit it for you. The authority \(Gluwacoin smart contract in this case\) can verify that the information in the contract is genuine with your signature.

You can create the signature using official Ethereum Web3 library, [web3.js](https://github.com/ethereum/web3.js) or [web3.py](https://github.com/ethereum/web3.py), or its unofficial implementation in other languages.

{% tabs %}
{% tab title="Javascript" %}
[web3.js](https://github.com/ethereum/web3.js)

{% code title="web3.js example" %}
```javascript
var Web3 = require('web3');

var web3 = new Web3(Web3.givenProvider);

// you will get the value below if you are successful
// '0x43a26051362b8040b289abe93334a5e3662751aa691185ae9e9a2e1e0c169350'
const messageHash = web3.utils.soliditySha3({t: 'string', v: "Some data"});

// example private key
const privateKey = '0x69965ac017a7d34820a029c6c815d81271d2b41878b85802975aa75a809c2e09';

const sign = web3.eth.accounts.sign(messageHash, privateKey);

const signature = sign.signature;

// you will get the value below if you are successful
// 0xc3d08236a76af3a25765a20de27edbc102197899482de8693118b7ffac95210005dc69be61e2838a4df72fe3fa76895cc3abb4c8957f8c000d6c4e1292c475ad1b
console.log(signature);
```
{% endcode %}

For JavaScript users, we also recommend using [ethers.js](https://github.com/ethers-io/ethers.js/) to create the signature. Below is an example of creating a signature for a transaction with your private key.

{% code title="ethers.js example" %}
```javascript
var ethers = require('ethers');

const messageHash = ethers.utils.solidityKeccak256(
    [ 'string' ], // types
    [ 'Some data' ] // values
);

const messageHashBinary = ethers.utils.arrayify(messageHash);

// example private key
const privateKey = '0x69965ac017a7d34820a029c6c815d81271d2b41878b85802975aa75a809c2e09'

let wallet = new ethers.Wallet(privateKey);

// creates a Promise with the Signature
let signPromise = wallet.signMessage(messageHashBinary);

// you will get the value below if you are successful
// 0xc3d08236a76af3a25765a20de27edbc102197899482de8693118b7ffac95210005dc69be61e2838a4df72fe3fa76895cc3abb4c8957f8c000d6c4e1292c475ad1b
signPromise.then(function (signature) {
    console.log(signature)
});
```
{% endcode %}
{% endtab %}
{% endtabs %}

### X-REQUEST-SIGNATURE

The Gluwa API uses `X-REQUEST-SIGNATURE` to authenticate transaction requests.

* A signed message from the user using the private key.
* The message is unix timestamp. You compare the `unixtimestamp` in the header string and the unix timestamp from the decrypted signature and compare.
* We use the public key to decrypt the signature and verify that the address actually belongs to the requester.
* The timestamp is used to check expiry of the signature. Expiry is 10 minutes.
* If this validation fails, you will get a `403` response.

#### Creating X-REQUEST-SIGNATURE

`X-REQUEST-SIGNATURE` is a base64 encoded string of `{unixtimestamp}.{signature}`

1. Get the current unix time stamp in seconds, `unixtimestamp`.
2. Using `unixtimestamp`as a message, create a `signature`. Refer to the **Signature** section above for details.
3. Concatenate `unixtimestamp` and `signature` separated with `.`. For example, if the `unixtimestamp` was "12345" and the `signature` was "asdf", you will get `12345.asdf`.
4. Base64 encoded the string you got from above to get `X-REQUEST-SIGNATURE`. You should get `MTIzNDUuYXNkZg==`.

### API Keys

The Gluwa API uses API keys to authenticate some types of requests. You can view and manage your API keys in the [Gluwa Dashboard](https://dashboard.gluwa.com).

The API key and secret will be passed to us using the following way:

* API Key Authorization Code: base64 encoded string of `{api key}:{api secret}`
* We use [HTTP Basic Auth](http://en.wikipedia.org/wiki/Basic_access_authentication) to use API Key authentication. Provide your API key as a header. `-H "Authorization: Basic {Base64 Encoded API Keys}"`.

{% code title="Authenticated Request" %}
```bash
$ curl https://api.gluwa.com/V1/QRCode \
  -H "Authorization: Basic {BASE64ENCODEDAPIKEY}"
```
{% endcode %}

#### Code Examples

{% tabs %}
{% tab title="Node.js" %}
```javascript
// example key and secret
var key = 'abcd';
var secret = '1234';
var data = key + ':' + secret;

var encodedBytes = Buffer.from(data);

// this is Base64 Encoded API Keys
var encodedString = encodedBytes.toString('base64');

// you should get 'YWJjZDoxMjM0' from the example values
console.log(encodedString)
```
{% endtab %}

{% tab title="Python" %}
```python
import base64

# example key and secret
key = 'abcd'
secret = '1234'

data = '%s:%s' % (key, secret)

encodedBytes = base64.b64encode(data.encode('utf-8'))

 # this is Base64 Encoded API Keys
encodedString = encodedBytes.decode('utf-8')

# you should get 'YWJjZDoxMjM0' from the example values
print(encodedString)
```
{% endtab %}
{% endtabs %}

## Core Resources

### Balance

This is an object representing the Bitcoin or Gluwacoin balance of a certain address. You can retrieve it to see the current balance on your Gluwa account.

You can also retrieve the nonce, which is used to sign the Ethereum contract.

#### The Balance Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Balance** | `string` | A string representing how much balance in the currency unit \(e.g., `"1.23"` for 1.23 USD-G\). |
| **Currency** | `string` | The currency unit of the balance: `"BTC"`, `"USDG"`, or `"KRWG"`. |
| **Nonce** | `long` | Used when signing Ethereum contract. Nonce is an integer that has to increase every time you make a transaction. For example, if you start from 0 for your very first transaction you make. The next transaction you make should be larger than 0. |

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

### Fee

To learn the recommended fee amount for a transaction, you can retrieve a `Fee` object. A blockchain charges transaction fees for processing  transaction \(e.g., [Bitcoin transaction fee](https://en.wikipedia.org/wiki/Bitcoin#Transaction_fees)\). Though transaction fees are optional, a transaction with higher fee gets processed first. Fee object includes the recommended minimum transaction fee, which is expected to get your transaction processed under a reasonable amount of time.

#### Fee Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Currency** | `string` | The currency unit of the fee |
| **MinimumFee** | `string` | The minimum recommended transaction fee  for the currency |

{% api-method method="get" host="https://api.gluwa.com" path="/v1/:currency/Fee" %}
{% api-method-summary %}
Retrieve a Fee for a Transaction
{% endapi-method-summary %}

{% api-method-description %}
Get recommended minimum fee for Bitcoin or Gluwacoin. The fee is dynamically calculated based on the current fee market.
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-path-parameters %}
{% api-method-parameter name="Currency" type="string" required=true %}
BTC, USDG or KRWG
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
    "Currency": "USDG",
    "MinimumFee": "0.5000"
}a
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid request
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
Service unavailable for this currency
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

### Transaction

To transfer funds to another user, you can create a `Transaction` object. You can retrieve an individual transaction as well as list all transactions. Transactions are identified by a unique, random `TxnHash`.

#### Transaction Object

| Attribute | Type | Description |
| :--- | :--- | :--- |
| **Status** | `string` | The status of the transaction:`"Unconfirmed"`, `"Confirmed"`, or `"Failed"` |
| **Amount** | `string` | The amount of the transaction. Negative if taken away from the source. If you are the sender, this is the amount deducted from your account. If you are the receiver, this is the amount added to your account. |
| **Fee** | `string` | _optional_ The transaction fee. For Gluwacoin, the fee is sum of Ethereum's gas fee and Gluwa's service fee. Omitted if you did not pay the fee. |
| **TotalAmount** | `string` | The sum of the `Amount` and the `Fee`. |
| **Currency** | `string` | The currency unit of the transaction |
| **Source** | `string[]` | The source of the transaction. For Bitcoin, may include null if it is a coinbase transaction. |
| **Target** | `string[]` | The target of the transaction. |
| **TxnHash** | `string` | Unique identifier of the transaction |
| **CreatedDateTime** | `datetime` | Time at which the object was created. |
| **ModifiedDateTime** | `datetime` | Time at which the object was last modified. |
| **MerchantOrderID** | `string` | _optional_  ****Used by the receiver to identify a payment. Supported by QR code payment feature. |
| **Note** | `string` | _optional_  Whatever message that the sender wanted to put. It is an optional memo you can associate with the transaction. |

#### Transaction Status

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
BTC, USDG or KRWG
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
		"Status": "Unconfirmed",
		"Amount": "1.00",
		"Fee": "0.50",
		"Currency": "USDG",
		"Source": string[],
		"Target": string[],
		"TxnHash": string,
		"CreatedDateTime": DateTime,
		"ModifiedDateTime": DateTime,
		"MerchantOrderID": string?,
		"Note": string?
	},
	{
		"Status": "Unconfirmed",
		"Amount": "1.00",
		"Fee": "0.50",
		"Currency": "USDG",
		"Source": string[],
		"Target": string[],
		"TxnHash": string,
		"CreatedDateTime": DateTime,
		"ModifiedDateTime": DateTime,
		"MerchantOrderID": string?,
		"Note": string?
	}
]
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid request. For example, the address is in an invalid format.
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Request signature header is not valid
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
Service unavailable
{% endapi-method-response-example-description %}

```

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
{% api-method-parameter name="Transaction Hash" type="string" required=true %}
The hash associated to the transaction
{% endapi-method-parameter %}

{% api-method-parameter name="Currency" type="string" required=true %}
BTC, USDG or KRWG
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
	Status: Unconfirmed, Confirmed, Failed
	Amount: string (negative if taken away from the source)
	Fee: string (gas fee + service fee)
	Currency: BTC, USDG, KRWG
	Source: string[]
	Target: string[]
	TxnHash: string
	CreatedDateTime: DateTime
	ModifiedDateTime: DateTime
	MerchantOrderID: string?
	Note: string?
}
```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid transaction hash format
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=404 %}
{% api-method-response-example-description %}
Transaction not found
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
Service unavailable
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

{% api-method method="post" host="https://api.gluwa.com" path="/v1/Transactions" %}
{% api-method-summary %}
Create a New Transaction
{% endapi-method-summary %}

{% api-method-description %}
Create a new transaction.  
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="PaymentID" type="string" required=false %}
A unique identifier for a payment. Supported by QR code payment feature.
{% endapi-method-parameter %}

{% api-method-parameter name="PaymentSig" type="string" required=false %}
Required if `PaymentID` is not null. Must be verified when submitted with `PaymentID`
{% endapi-method-parameter %}

{% api-method-parameter name="Signature" type="string" required=true %}
A signature of the body parameters signed with the sender's private key. See "**How to Create a Signature for Create Transaction Endpoint**" below for details.
{% endapi-method-parameter %}

{% api-method-parameter name="Source" type="string" required=true %}
Address of the sender
{% endapi-method-parameter %}

{% api-method-parameter name="Currency" type="string" required=true %}
BTC, USDG or KRWG
{% endapi-method-parameter %}

{% api-method-parameter name="Target" type="string" required=true %}
Address of the receiver
{% endapi-method-parameter %}

{% api-method-parameter name="Amount" type="string" required=true %}
This is the amount that the receiver receives.  If you want to send 1.85 BTC, put "1.85" here.
{% endapi-method-parameter %}

{% api-method-parameter name="Fee" type="string" required=true %}
This is the transaction fee amount that the sender pays. If you want to pay 0.01 BTC for transaction fee, put "0.01" here. Note that there is a recommended transaction fee amount and you can get it from **Retrieve a Fee for a Transaction** endpoint. Your transaction may not get processed by Gluwa if the fee amount is smaller than the recommended amount.
{% endapi-method-parameter %}

{% api-method-parameter name="Nonce" type="string" required=false %}
Used when signing Ethereum contract. Nonce is an integer that has to increase every time you make a transaction. For example, if you start from 0 for your very first transaction you make. The next transaction you make should be larger than 0. You can find your current nonce when you call **Retrieve Balance for an Address** endpoint for Gluwacoin addresses.
{% endapi-method-parameter %}

{% api-method-parameter name="MerchantOrderID" type="string" required=false %}
A user-determined value which is used to identify the purpose of the transaction. Supported by QR code payment feature.
{% endapi-method-parameter %}

{% api-method-parameter name="Note" type="string" required=false %}
Whatever message that the sender wanted to put. It is an optional memo you can associate with the transaction.
{% endapi-method-parameter %}

{% api-method-parameter name="Idem" type="string" required=false %}
Used to prevent submitting duplicate requests. For example, when you submit a request and the server accepted the request but for some reason the server returns an error, you will retry with the exactly the same request. The server will fail this duplicated request.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=202 %}
{% api-method-response-example-description %}
The request has been accepted for processing, but the processing does not get completed immediately. Gluwa does not return the transaction hash immediately since it is not determined until we send the transaction information to the blockchain.
{% endapi-method-response-example-description %}

```javascript
{}

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Invalid request. Validation error. See inner errors for more details. Signed BTC transaction could not be verified
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=409 %}
{% api-method-response-example-description %}
A transaction with the same transaction hash, payment ID, or idem already exists.
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
Service unavailable
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

#### ETHless Transfer

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

#### Gluwacoin Contracts

* USD Gluwacoin - [0xfb0aaa0432112779d9ac483d9d5e3961ece18eec](https://etherscan.io/token/0xfb0aaa0432112779d9ac483d9d5e3961ece18eec)
* KRW Gluwacoin - [0x4cc8486f2f3dce2d3b5e27057cf565e16906d12d](https://etherscan.io/token/0x4cc8486f2f3dce2d3b5e27057cf565e16906d12d)

### QR Code

Instead of typing a long address, a user may scan [QR code](https://en.wikipedia.org/wiki/QR_code) to initiate a transfer on Gluwa mobile application.

#### Types of QR Code

Gluwa has two types of QR code:

* **Transfer QR Code**: A user can generate Transfer QR code using Gluwa mobile application. If you scan the QR code using Gluwa mobile application, you initiate a transfer to the user. This QR code initiates an exact amount transfer. So you see the fee information on the transaction summary page.
* **Payment QR Code**: A user can generate Payment QR code using Gluwa API. The user needs to obtain API keys to do this. This QR code forces the receiver to pay the fee. This means that Gluwa mobile application does not show the fee information on the transaction summary page.

{% api-method method="post" host="https://api.gluwa.com" path="/v1/QRCode" %}
{% api-method-summary %}
Create a Payment QR Code
{% endapi-method-summary %}

{% api-method-description %}
Create a Payment QR Code
{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-headers %}
{% api-method-parameter name="Content-Type" type="string" required=true %}
application/json
{% endapi-method-parameter %}

{% api-method-parameter name="Authorization" type="string" required=true %}
Gluwa uses API keys and basic access authentication to authenticate this endpoint. Refer to "**API Keys**" in the Authentication section for details.
{% endapi-method-parameter %}
{% endapi-method-headers %}

{% api-method-query-parameters %}
{% api-method-parameter name="format" type="string" required=false %}
When the parameter is omitted, the endpoint returns Base64. Pass `image/png` for PNG or `image/jpeg` for JPEG. 
{% endapi-method-parameter %}
{% endapi-method-query-parameters %}

{% api-method-body-parameters %}
{% api-method-parameter name="Signature" type="string" required=true %}
A signature to verify if the request was made within 10 minutes from now by the owner of the target address. See "**How to Create a Signature for Create a Payment QR Code Endpoint**" below for details.
{% endapi-method-parameter %}

{% api-method-parameter name="Currency" type="string" required=true %}
USDG or KRWG
{% endapi-method-parameter %}

{% api-method-parameter name="Target" type="string" required=true %}
Address of the receiver
{% endapi-method-parameter %}

{% api-method-parameter name="Amount" type="string" required=true %}
Amount of the transfer initiated by the QR code
{% endapi-method-parameter %}

{% api-method-parameter name="MerchantOrderID" type="string" required=false %}
_Limited to 60 characters._ A user-determined value which is used to identify the purpose of the transaction. Supported by QR code payment feature.
{% endapi-method-parameter %}

{% api-method-parameter name="Note" type="string" required=false %}
_Limited to 280 characters._ Whatever message that the sender wanted to put. It is an optional memo you can associate with the transaction.
{% endapi-method-parameter %}

{% api-method-parameter name="Expiry" type="integer" required=false %}
The lifetime of the QR code in seconds. For example, you can put \`300\` to get a QR code that will expire 5 minutes from now.
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}
Returns a QR code image in the requested format \(Base64 by default and JPG or PNG if requested\). You can scan the code with Gluwa mobile app to initiate the payment transaction.
{% endapi-method-response-example-description %}

```javascript

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=400 %}
{% api-method-response-example-description %}
Validation Error: Validation error. Please see inner errors for more details. Bad Request Error: API Key and secret request header is missing or invalid
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}

{% api-method-response-example httpCode=403 %}
{% api-method-response-example-description %}
Combination of Api Key and Api Secret was not found
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
Service unavailable for the provided currency
{% endapi-method-response-example-description %}

```

```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

#### How to Create a Signature for Create Payment QR Code Endpoint

Learn how to create the `Signature` from the Body Parameters from the example below.

{% tabs %}
{% tab title="Node.js" %}
```javascript
var ethers = require('ethers');

// example private key
let key = '6af4b282a849d9e6fee02d63867285c3de1167ac445d6807e673d3c3749414b7';
let wallet = new ethers.Wallet(key);

// example timestamp
let timestamp = '1568414920';
// example code to get Unix timestamp
// let timestamp = Math.round((new Date()).getTime() / 1000).toString();

// creates a Promise with the Signature
let signPromise = wallet.signMessage(timestamp);

signPromise.then((signedTransaction) => {
    // you should get below from the example values
    // 0xf5ccf41438561cc58d4efba9d944a01f8e7f27258091280154a8c48fea14521c3027e47e10d87328c873304975b8c30d152a50a313f2e22b470f59f49df08dde1c
    console.log(signedTransaction);
    
    var data = timestamp + '.' + signedTransaction;
    
    var encodedBytes = Buffer.from(data);

    // this is Base64 Encoded API Keys
    var encodedString = encodedBytes.toString('base64');

    // you should get below from the example values
    // MTU2ODQxNDkyMC4weGY1Y2NmNDE0Mzg1NjFjYzU4ZDRlZmJhOWQ5NDRhMDFmOGU3ZjI3MjU4MDkxMjgwMTU0YThjNDhmZWExNDUyMWMzMDI3ZTQ3ZTEwZDg3MzI4Yzg3MzMwNDk3NWI4YzMwZDE1MmE1MGEzMTNmMmUyMmI0NzBmNTlmNDlkZjA4ZGRlMWM=
    console.log(encodedString);
});
```
{% endtab %}
{% endtabs %}



