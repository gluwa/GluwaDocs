# QR Code

Instead of typing a long address, a user may scan [QR code](https://en.wikipedia.org/wiki/QR_code) to initiate a transfer on Gluwa mobile application.

## Types of QR Code

Gluwa has two types of QR code:

* **Transfer QR Code**: A user can generate Transfer QR code using Gluwa mobile application. If you scan the QR code using Gluwa mobile application, you initiate a transfer to the user. This QR code initiates an exact amount transfer. So you see the fee information on the transaction summary page.
* **Payment QR Code**: A user can generate Payment QR code using Gluwa API. The user needs to obtain API keys to do this. This QR code forces the receiver to pay the fee. This means that Gluwa mobile application does not show the fee information on the transaction summary page.

{% api-method method="post" host="https://api.gluwa.com" path="/v1/QRCode" %}
{% api-method-summary %}
Create Payment QR Code
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

