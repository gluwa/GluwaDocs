---
description: Learn various authorization methods you need to use Gluwa API.
---

# Authorization

Gluwa has two types of authorizing a request:

1. `X-REQUEST-ENDPOINT` header
2. `Authorization` header

Depending on the request, you may have to use at least one of them or none at all. Look under **Request -&gt; Headers** section under each endpoint to find out if an endpoint requires authorization.

## X-REQUEST-SIGNATURE

`X-REQUEST-SIGNATURE` header is used to verify the ownership of an address, usually, for `GET` requests. The value of the header must be the signature of an address that you own. Follow the guide below to generate an Address Signature. 

{% page-ref page="../development/todo-creating-transaction-signatures.md" %}



Then, you can generate the value of the header like below:

```text
Base64Encode(<unix timestamp>.<Address Signature>)
```

So for example,

{% tabs %}
{% tab title="Gluwacoin \(ex> USDG, KRWG, NGNG\)" %}
```text
// Given 3 values below
unix timestamp = 1587674497
public address = 0x3E6d16c11497aD1A2F47a6594d995f1FaaE727d9
private key = 18cffe0cd4eb63809d0e55ed8dd1aa29e3ac660088e82f7a82977c458f334d8b


// Address Signature
Address Signature = 0x96322ca1b963c98e33fe1296b504d3c7adfcfd4e8473bf92f6ee24b560497d16390404a4f9f241d9efdd02cf1fea79d0ebf45d4aa2ef47a4c97fa06750e242301c


// Value of X-REQUEST-SIGNATURE header
X-REQUEST-SIGNATURE = Base64Encode("1587674497.0x96322ca1b963c98e33fe1296b504d3c7adfcfd4e8473bf92f6ee24b560497d16390404a4f9f241d9efdd02cf1fea79d0ebf45d4aa2ef47a4c97fa06750e242301c")
                    = MTU4NzY3NDQ5Ny4weDk2MzIyY2ExYjk2M2M5OGUzM2ZlMTI5NmI1MDRkM2M3YWRmY2ZkNGU4NDczYmY5MmY2ZWUyNGI1NjA0OTdkMTYzOTA0MDRhNGY5ZjI0MWQ5ZWZkZDAyY2YxZmVhNzlkMGViZjQ1ZDRhYTJlZjQ3YTRjOTdmYTA2NzUwZTI0MjMwMWM=


```
{% endtab %}

{% tab title="BTC" %}
```text
// Given 3 values below
unix timestamp = 1587674497
public address = 12koEsMzrdxuZ71ATU1a5jgZyUYtf3debA
private key = KwJfd6xHiqtEFBawy8tKPyJ9TFKQCqHpMr8DQVJ9LbUBj21jqFjE


// Address Signature
Address Signature = H8Gc4g7/X+JsHZyV/qjQSMg9ivoopMztzx9efeV+a+eAJ7Y45OnEi3qmhVWaL743jofge4gQVapzAVsHFSSpBSk=


// Value of X-REQUEST-SIGNATURE header
X-REQUEST-SIGNATURE = Base64Encode("1587674497.H8Gc4g7/X+JsHZyV/qjQSMg9ivoopMztzx9efeV+a+eAJ7Y45OnEi3qmhVWaL743jofge4gQVapzAVsHFSSpBSk=")
                    = MTU4NzY3NDQ5Ny5IOEdjNGc3L1grSnNIWnlWL3FqUVNNZzlpdm9vcE16dHp4OWVmZVYrYStlQUo3WTQ1T25FaTNxbWhWV2FMNzQzam9mZ2U0Z1FWYXB6QVZzSEZTU3BCU2s9


```
{% endtab %}
{% endtabs %}

There are couple things to note:

1. Make sure that unix timestamp is in seconds, **NOT** milliseconds.
2. The generated `X-REQUEST-SIGNATURE` will be valid for 10 minutes. After that, any request made with the same header value will return 403 response.

## API Keys and Secrets

Some endpoints use API keys and secrets to authorize the request. You can view and manage your API key and secrets in [Gluwa Dashboard](https://dashboard.gluwa.com).

We use [Basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication) scheme. 

```text
Token = Base64Encode("<api key>:<api secret>")
Authorization Header value = "Basic <Token>"
```

For example, you would use call an endpoint like below using curl.

{% code title="Authenticated Request" %}
```bash
$ curl https://api.gluwa.com/my/gluwa/endpoint \
  -H "Authorization: Basic {Token}"
```
{% endcode %}

### Code Examples

{% tabs %}
{% tab title="JavaScript" %}
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

{% tab title="C\#" %}
```csharp
using System;
using System.Text;

...

string apiKey = "abcd";
string apiSecret = "1234";

// token's value is YWJjZDoxMjM0
string token = Convert.ToBase64String(Encoding.UTF8.GetBytes($"{apiKey}:{apiSecret}"));
```
{% endtab %}
{% endtabs %}

