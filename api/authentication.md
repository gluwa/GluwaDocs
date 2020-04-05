---
description: Learn various authentication methods you need to use the Gluwa API.
---

# Authentication

## HTTPS

All API requests must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTP_Secure). Calls made over plain HTTP will fail. API requests without proper authentication will also fail.

## Signature

Signature is used to verify the ownership of a cryptocurrency \(e.g., [Bitcoin](https://en.wikipedia.org/wiki/Bitcoin) or [Ethereum](https://en.wikipedia.org/wiki/Ethereum)\) wallet. A cryptocurrency wallet is a set of a private key and a public key. You can verify the ownership of a cryptocurrency wallet by encrypting an arbitrary message with its private key. Anyone who has the public key can decrypt the encryption with it and get the original text.

### Gluwacoin Signature

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

### Bitcoin Signature

To verify the ownership of a Bitcoin wallet, you need to create a Bitcoin signature.

You can create the signature using an unofficial Bitcoin library, [BitcoinJS](https://www.npmjs.com/package/bitcoinjs-lib).

```javascript
import bitcoin from 'bitcoinjs-lib';

const satoshi = require('satoshi-bitcoin');
const bigInt = require("big-integer");
const network = bitcoin.networks.bitcoin; // If you are developing on a testnet, using 'bitcoin.networks.testnet'
const keyPair = bitcoin.ECPair.fromWIF(privateKey, network);
const tx = new bitcoin.TransactionBuilder(network);
const unspentOutputs = [
    {Amount: "0.00022", TxHash: "b491c98630ab1149ee584fb79c5097fc53b1d37cc0ffc44cfa379cc53a10ffc9", Index: 1, Confirmations: 1878},
    {Amount: "0.01383123", TxHash: "f26b39d58c283c7bcb1d8a70d8cb55cef20934f964b2314dea1955e63d9027a7", Index: 0, Confirmations: 1030}
]; // This is example. You can get this from our getAddresses API. (/V1/${currency}/Addresses/${address}?includeUnspentOutputs=true)
const args = {
    sendersAddress: '...', // Sender's Address
    receiversAddress: '...', // Receiver's Address
    amount: 0.003, // example. The amount of BTC you are going to transfer.
    fee: 0.0005, // You can get this from our getFee API. (/V1/${currency}/Fee)
};

const amount = bigInt(satoshi.toSatoshi(args.amount));
const fee = bigInt(satoshi.toSatoshi(args.fee));
const difference = sumInput.subtract(amount).subtract(fee);

tx.addInput(unspentOutputs[i].TxHash, unspentOutputs[i].Index); // This line should be repeated until the sum of the values ​​of unspentOutputs is greater than Amount.
tx.addOutput(args.sendersAddress, difference.toJSNumber());
tx.addOutput(args.receiversAddress, amount.toJSNumber());
tx.sign(unspentOutputs[i].Index, keyPair); // This line must be repeated as long as addInput is repeated, and the index used must be re-entered.
```

## X-REQUEST-SIGNATURE

The Gluwa API uses `X-REQUEST-SIGNATURE` to authenticate transaction requests.

* A signed message from the user using the private key.
* The message is unix timestamp. You compare the `unixtimestamp` in the header string and the unix timestamp from the decrypted signature and compare.
* We use the public key to decrypt the signature and verify that the address actually belongs to the requester.
* The timestamp is used to check expiry of the signature. Expiry is 10 minutes.
* If this validation fails, you will get a `403` response.

### Creating X-REQUEST-SIGNATURE

`X-REQUEST-SIGNATURE` is a base64 encoded string of `{unixtimestamp}.{signature}`

1. Get the current unix time stamp in seconds, `unixtimestamp`.
2. Using `unixtimestamp`as a message, create a `signature`. Refer to the **Signature** section above for details.
3. Concatenate `unixtimestamp` and `signature` separated with `.`. For example, if the `unixtimestamp` was "12345" and the `signature` was "asdf", you will get `12345.asdf`.
4. Base64 encoded the string you got from above to get `X-REQUEST-SIGNATURE`. You should get `MTIzNDUuYXNkZg==`.

## API Keys

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

