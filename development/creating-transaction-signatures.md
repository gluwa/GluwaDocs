# Creating Signatures

## Signature

A signature helps us to verify that a piece of data was signed by a specific wallet account. There are two types of signatures used in Gluwa API:

1. Address Signature
2. Transaction Signature

## Address Signature

Address signature is used to verify the ownership of a cryptocurrency \(e.g., [Bitcoin](https://en.wikipedia.org/wiki/Bitcoin) or [Ethereum](https://en.wikipedia.org/wiki/Ethereum)\) wallet. A cryptocurrency wallet is a set of a private key and a public key. To generate a signature, you sign a message \(or a piece of data\) with your private key, and then anyone with your public key can verify the integrity of your signature. To prevent the same signature used over and over again, our APIs mandates the users to use Unix timestamp as the message. The server will check if the timestamp was created no more than 10 minutes ago, and if it has been over 10 minutes, it will deny the request.

### Generating Address Signature for Gluwacoins \(USD-G, KRW-G, NGN-G\)

Since Gluwacoins, such as USD-G, KRW-G and NGN-G, are ERC20 tokens, you can sign a message just like you would on any Ethereum based address. There are libraries out there that can help you create signatures. Some examples include:

1. [web3.js](https://github.com/ethereum/web3.js). \(JavaScript\)
2. [Nethereum](https://github.com/Nethereum/Nethereum/) \(C\#\)

{% tabs %}
{% tab title="JavaScript \(web.js\)" %}
```javascript
var Web3 = require('web3');

...

// replace with your own message and privakey
var message = "1587674497";
var privateKey = "0x18cffe0cd4eb63809d0e55ed8dd1aa29e3ac660088e82f7a82977c458f334d8b";

var web3 = new Web3(Web3.givenProvider);
var obj = web3.eth.accounts.sign(message , privateKey);

// signature: 0x96322ca1b963c98e33fe1296b504d3c7adfcfd4e8473bf92f6ee24b560497d16390404a4f9f241d9efdd02cf1fea79d0ebf45d4aa2ef47a4c97fa06750e242301c
var signature = obj.signature;


```
{% endtab %}

{% tab title="C\# \(Nethereum\)" %}
```csharp
using Nethereum.Signer;

...

// replace with your own message and privakey
string message = "1587674497";
string privateKey = "0x18cffe0cd4eb63809d0e55ed8dd1aa29e3ac660088e82f7a82977c458f334d8b";

EthereumMessageSigner signer = new EthereumMessageSigner();

// signature: 0x96322ca1b963c98e33fe1296b504d3c7adfcfd4e8473bf92f6ee24b560497d16390404a4f9f241d9efdd02cf1fea79d0ebf45d4aa2ef47a4c97fa06750e242301c
string signature = signer.EncodeUTF8AndSign(message, new EthECKey(privateKey));


```
{% endtab %}
{% endtabs %}

{% hint style="warning" %}
In production environment, your private key should never be hard coded like above. Private keys must be kept secret and be handled with utmost care.
{% endhint %}

### Generating Address Signature for Bitcoin

There are unofficial Bitcoin libraries you can use to create the signature. Some examples include:

1. [BitcoinJS](https://github.com/bitcoinjs/bitcoinjs-lib) \(JavaScript\)
2. [NBitcoin](https://github.com/MetacoSA/NBitcoin) \(C\#\)

{% tabs %}
{% tab title="BitcoinJS \(JavaScript\)" %}
```javascript
var bitcoin = require('bitcoinjs-lib');
var bitcoinMessage = require('bitcoinjs-message');

...

// replace with your own message and privakey
var network = bitcoin.networks.bitcoin;
var message = "1587674497";
var privateKeyString = "KwJfd6xHiqtEFBawy8tKPyJ9TFKQCqHpMr8DQVJ9LbUBj21jqFjE";

var keyPair = bitcoin.ECPair.fromWIF(privateKeyString, network);
var privateKey = keyPair.privateKey;

// signature: H8Gc4g7/X+JsHZyV/qjQSMg9ivoopMztzx9efeV+a+eAJ7Y45OnEi3qmhVWaL743jofge4gQVapzAVsHFSSpBSk=
var signature = bitcoinMessage
        .sign(message, privateKey, keyPair.compressed)
        .toString('base64');


```
{% endtab %}

{% tab title="NBitcoin \(C\#\)" %}
```csharp
using NBitcoin;

...

// replace with your own message and privakey
string message = "1587674497";
string privateKey = "KwJfd6xHiqtEFBawy8tKPyJ9TFKQCqHpMr8DQVJ9LbUBj21jqFjE";

BitcoinSecret secret = Network.Main.CreateBitcoinSecret(privateKey);

// signature: H1iG0zN+TmigcsHDUw2XQo0Wd0LKXbDVG8yr104I0g64cB1kq6WUPKUiI1oNc9Uo9hCRFlfZ3UcALJZNCSy1ZjY=
string signature = secret.PrivateKey.SignMessage(message);

// OR you don't have to force low R. Both format is accepted
// signature: H8Gc4g7/X+JsHZyV/qjQSMg9ivoopMztzx9efeV+a+eAJ7Y45OnEi3qmhVWaL743jofge4gQVapzAVsHFSSpBSk=
signature = secret.PrivateKey.SignMessage(Encoding.UTF8.GetBytes(message), false);


```
{% endtab %}
{% endtabs %}

## Transaction Signature

To create a transaction, you must create a signature of the transfer information so that Gluwa can submit it to the blockchain as a proof-of-intention. Think of signature as a signed contract. You send the contract to Gluwa to submit it to the blockchain for you. The authority \(Gluwacoin smart contract or Bitcoin blockchain in this case\) can verify that the information in the contract is genuine with your signature.

### Generating Transaction Signature for Gluwacoins \(USD-G, KRW-G, NGN-G\)

Gluwacoin is an [ERC-20](https://en.wikipedia.org/wiki/ERC-20) token. Users can transfer Gluwacoin as they would transfer any other ERC-20 token. To move tokens on the blockchain, you would normally have to spend Ether to cover the gas for the transaction. However, Gluwacoin allows you to move the coins by paying gas in Gluwacoin instead of Ether. We call this _ETHless transfer_.

You are going to need the contract addresses to make a transaction signature. Use the following addresses.

{% tabs %}
{% tab title="Production" %}
**ERC20 Contract Addresses**

* USD-G Gluwacoin - [0xfb0aaa0432112779d9ac483d9d5e3961ece18eec](https://etherscan.io/token/0xfb0aaa0432112779d9ac483d9d5e3961ece18eec)
* KRW-G Gluwacoin - [0x4cc8486f2f3dce2d3b5e27057cf565e16906d12d](https://etherscan.io/token/0x4cc8486f2f3dce2d3b5e27057cf565e16906d12d)
* NGN-G Gluwacoin - [0x4AB30B965A8Ef0F512DA064B5e574d9Ad73c0e79](https://etherscan.io/token/0x4AB30B965A8Ef0F512DA064B5e574d9Ad73c0e79)
{% endtab %}

{% tab title="Sandbox" %}
**ERC20 Contract Addresses**

* USD-G Gluwacoin - [0x8e9611f8ebc9323edda39ea2d8f31bbb2436adee](https://rinkeby.etherscan.io/token/0x8e9611f8ebc9323edda39ea2d8f31bbb2436adee)
* KRW-G Gluwacoin - [0x408b7959b3e15b8b1e8495fa9cb123c0180d44db](https://rinkeby.etherscan.io/token/0x408b7959b3e15b8b1e8495fa9cb123c0180d44db)
* NGN-G Gluwacoin - [0x1aa950bd468997a28927434cb4f030ae0f19c8a7](https://rinkeby.etherscan.io/token/0x1aa950bd468997a28927434cb4f030ae0f19c8a7)
{% endtab %}
{% endtabs %}

To create a Transaction Signature, you can use well known libraries like [web.js](https://github.com/ethereum/web3.js) or [Nethereum](https://github.com/Nethereum/Nethereum/).

{% tabs %}
{% tab title="web3.js \(JavaScript\)" %}
```javascript
var Web3 = require('web3');

...

// replace the values below with your own values
var contractAddress = "0xfb0aaa0432112779d9ac483d9d5e3961ece18eec"; // USD-G contract
var sourceAddress = "0x3E6d16c11497aD1A2F47a6594d995f1FaaE727d9";
var sourcePrivateKey = "0x18cffe0cd4eb63809d0e55ed8dd1aa29e3ac660088e82f7a82977c458f334d8b";
var targetAddress = "0xc4f7fDf5EB4a1204dCa3BeC609f9E457C4fF9844";
var amount = 100; // sending 100 USD-G
var fee = 0.5; // fee is 0.5 USD-G
var nonce = 1;

var web3 = new Web3(Web3.givenProvider);

var hash = web3.utils.soliditySha3({ t: 'address', v: contractAddress },
    { t: 'address', v: sourceAddress },
    { t: 'address', v: targetAddress },
    { t: 'uint256', v: web3.utils.toWei(amount.toString(), 'ether') },
    { t: 'uint256', v: web3.utils.toWei(fee.toString(), 'ether') },
    { t: 'uint256', v: nonce });
    
var obj = web3.eth.accounts.sign(hash , sourcePrivateKey);

// signature: 0x8fa899ec93a3f20b5294bed40ab33bac0913b0c9670a48795b99ff8f994526b95e6ff5d7e8ef2ab99c4b1edaf70569b549ba3747e7b4e43ade28405035777bfb1b
var signature = obj.signature;



```
{% endtab %}

{% tab title="Nethereum \(C\#\)" %}
```csharp
using Nethereum.ABI;
using Nethereum.Signer;
using Nethereum.Util;
using Nethereum.Web3;
using System.Numerics;

...

// replace the values below with your own values
string contractAddress = "0xfb0aaa0432112779d9ac483d9d5e3961ece18eec"; // USD-G contract
string sourceAddress = "0x3E6d16c11497aD1A2F47a6594d995f1FaaE727d9";
string sourcePrivateKey = "0x18cffe0cd4eb63809d0e55ed8dd1aa29e3ac660088e82f7a82977c458f334d8b";
string targetAddress = "0xc4f7fDf5EB4a1204dCa3BeC609f9E457C4fF9844";
decimal amount = 100m; // sending 100 USD-G
decimal fee = 0.5m; // fee is 0.5 USD-G
BigInteger nonce = new BigInteger(1);


ABIEncode abiEncode = new ABIEncode();

byte[] hash = abiEncode.GetSha3ABIEncodedPacked(
                new ABIValue("address", contractAddress),
                new ABIValue("address", sourceAddress),
                new ABIValue("address", targetAddress),
                new ABIValue("uint256", Web3.Convert.ToWei(amount, UnitConversion.EthUnit.Ether)),
                new ABIValue("uint256", Web3.Convert.ToWei(fee, UnitConversion.EthUnit.Ether)),
                new ABIValue("uint256", nonce)
                );

EthereumMessageSigner messageSigner = new EthereumMessageSigner();

// signature: 0x8fa899ec93a3f20b5294bed40ab33bac0913b0c9670a48795b99ff8f994526b95e6ff5d7e8ef2ab99c4b1edaf70569b549ba3747e7b4e43ade28405035777bfb1b
string signature = messageSigner.Sign(hash, sourcePrivateKey );

```
{% endtab %}
{% endtabs %}

{% hint style="info" %}
Note that when you generate the transaction signature, you must multiply the amount and the fee by 10^18 \(1,000,000,000,000,000,000\). For example, if you want to transfer 10 USD-G, the value for the amount when you create transaction signature must be 10,000,000,000,000,000,000. However, the amount in the transaction request body must be 10.
{% endhint %}

### Generating Transaction Signature for Bitcoin

Create and sign Bitcoin transaction like you normally would. You can use well known libraries like [BitcoinJS](https://github.com/bitcoinjs/bitcoinjs-lib) or [NBitcoin](https://github.com/MetacoSA/NBitcoin).

{% tabs %}
{% tab title="BitcoinJS \(JavaScript\)" %}
```javascript
var bitcoin = require('bitcoinjs-lib');
var bitcoinMessage = require('bitcoinjs-message');

...


var btcToSatoshi = 100000000;

// replace the values below with your own values
var network = bitcoin.networks.bitcoin;
var sourceAddressString = "12koEsMzrdxuZ71ATU1a5jgZyUYtf3debA";
var sourceAddressPrivateKey = "KwJfd6xHiqtEFBawy8tKPyJ9TFKQCqHpMr8DQVJ9LbUBj21jqFjE";
var targetAddressString = "14NYL22gWLrVPEdjM6W5oJXcWyu496kZCQ";
var amountToSend = 0.001;
var fee = 0.00006;
var previousTxnHash = "e52fd66f35c48e690d8caada1a881ebb11912c479783f51f839a8827a9afcad4";
var unspentOutputAmount = 0.03800757;
var unspentOutputIndex = 1;
    
var keyPair = bitcoin.ECPair.fromWIF(sourceAddressPrivateKey, network);
   
var txnBuilder = new bitcoin.TransactionBuilder(network);
txnBuilder.addInput(previousTxnHash, unspentOutputIndex);
txnBuilder.addOutput(sourceAddressString, (unspentOutputAmount - amountToSend - fee) * btcToSatoshi);
txnBuilder.addOutput(targetAddressString, amountToSend * btcToSatoshi);
txnBuilder.sign(0, keyPair);

var txn = txnBuilder.build();

// signature: 0200000001d4caafa927889a831ff58397472c9111bb1e881adaaa8c0d698ec4356fd62fe5010000006b483045022100f5e8970b5cb154e96d5696f8fa2c36fabb3e86caf2cd681490448b38403ee92b0220797a0f61f869173d4dbf65894b772cb953349defe2b6275872f0a0b3bc01b86a0121034274d104b6d68ebf109ae3cd35cd53e8b8cd359bedf5598a92033292ae7f76a4ffffffff02a5603800000000001976a91413409c509b056480b21e54d0c5d2b87965eff65188aca0860100000000001976a91424fb438587b31ffd7612f4fbf6d67e21f4bb3ec788ac00000000
var signature = txn.toHex();


```
{% endtab %}

{% tab title="NBitcoin \(C\#\)" %}
```csharp
using NBitcoin;

...

// replace the values below with your own values
Network network = Network.Main;
string sourceAddressString = "12koEsMzrdxuZ71ATU1a5jgZyUYtf3debA";
string sourceAddressPrivateKey = "KwJfd6xHiqtEFBawy8tKPyJ9TFKQCqHpMr8DQVJ9LbUBj21jqFjE";
string targetAddressString = "14NYL22gWLrVPEdjM6W5oJXcWyu496kZCQ";
decimal amountToSend = 0.001m;
decimal fee = 0.00006m;
string previousTxnHash = "e52fd66f35c48e690d8caada1a881ebb11912c479783f51f839a8827a9afcad4";
string previousTxnScriptPubKey = "76a914b797bc584c969a2b6b5e613c5ab86b5de57c07ec88ac";
Money unspentOutputAmount = Money.Parse("0.03800757");
uint unspentOutputIndex = 1;

BitcoinAddress sourceAddress = BitcoinAddress.Create(sourceAddressString, network);
BitcoinAddress targetAddress = BitcoinAddress.Create(targetAddressString, network);
BitcoinSecret secret = new BitcoinSecret(sourceAddressPrivateKey, network);

TransactionBuilder builder = network.CreateTransactionBuilder();
Transaction txn = builder
                .AddKeys(secret)
                .AddCoins(
                    new Coin(
                        fromTxHash: new uint256(previousTxnHash),
                        fromOutputIndex: unspentOutputIndex,
                        amount: unspentOutputAmount,
                        scriptPubKey: Script.FromHex(previousTxnScriptPubKey)
                    )
                )
                .Send(targetAddress, amountToSend.ToString())
                .SetChange(sourceAddress)
                .SendFees(fee.ToString())
                .BuildTransaction(true);

// signature: 0100000001d4caafa927889a831ff58397472c9111bb1e881adaaa8c0d698ec4356fd62fe50100000000ffffffff02a0860100000000001976a91424fb438587b31ffd7612f4fbf6d67e21f4bb3ec788aca5603800000000001976a91413409c509b056480b21e54d0c5d2b87965eff65188ac00000000
string signature = txn.ToHex();


```
{% endtab %}
{% endtabs %}

Note that the above is just an example. The number of inputs and outputs may differ for each transaction so you may have to rearrange inputs and outputs accordingly.

