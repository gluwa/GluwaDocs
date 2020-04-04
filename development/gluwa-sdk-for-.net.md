# Gluwa SDK for .NET

If your service is developed in .NET, the features we provide are available through the SDK. The Gluwa SDK for .NET is a library with powerful features that enable .NET developers to easily make requests to the Gluwa APIs.

## Getting Started

The namespace of Gluwa SDK is [`Gluwa.SDK_dotnet`](https://www.nuget.org/packages/Gluwa.SDK_dotnet/). The SDK requires **.NET Core 2.0** or above.

{% embed url="https://www.nuget.org/packages/Gluwa.SDK\_dotnet/" %}

Refer to the Microsoft documentation below to learn how to install Nuget packages.

{% embed url="https://docs.microsoft.com/nuget/install-nuget-client-tools" %}

## Clients

To use Gluwa API, you need to create a `Client` object and initialize it. There are two types of Clients:

1. `GluwaClient` for managing funds by creating or retrieving transaction
2. `QRCodeClient` for creating QR codes.

{% tabs %}
{% tab title="Input" %}
| **Type** | **Description** |
| :--- | :--- |
| **bool** |  `false` by default. If you want to use the SandBox mode, set `true` |
{% endtab %}
{% endtabs %}

### GluwaClient

```csharp
GluwaClient gluwaClient = new GluwaClient();
// If you want to use the SandBox mode
GluwaClient gluwaClient = new GluwaClient(true);
```

### QRCodeClient

```csharp
QRCodeClient qrCodeClient = new QRCodeClient();
// If you want to use the SandBox mode
QRCodeClient qrCodeClient = new QRCodeClient(true);
```

## Method Examples

### GluwaClient

#### [Create a New Transaction](../api/api.md#create-a-new-transaction)

```csharp
ECurrency currency = “{USDG or KRWG}”;
string address = “{Your Gluwacoin public Address}”;
string privateKey = “{Your Gluwacoin Private Key}”;
string amount = “{Transaction amount, not including the fee}”;
string target = “{The address that the transaction will be sent to}”
string merchantOrderID = “{Identifier for the transaction that was provided by the merchant user}”; //default to null. Optional
string note = “{Additional information about the transaction that a user can provide}”; //default to null. Optional”

Result<bool, ErrorResponse> result = await gluwaClient.CreateTransactionAsync(
    currency, 
    address, 
    privateKey, 
    amount, 
    target, 
    merchantOrderID, // optional, default = null
    note // optional, default = null
);

if (result.IsFailure)
{
    switch (result.Error.Code)
    {
        case "ErrorCode1":
            // handle error 1
            break;
        case "ErrorCode2":
            // handle error 2
            break;
        default:
            // handle error
            break;
    }
}
else
{
    // successful response. See result.Data for the response
}
```

Returns `true` if successful and `false` if unsuccessful. Successful response means that the transaction has been accepted by Gluwa and will be included in the blockchain in couple of minutes.

#### [List Transaction History for an Address](../api/api.md#list-transaction-history-for-an-address)

```csharp
ECurrency currency = “{USDG or KRWG}”;
string address = “{Your Gluwacoin public Address}”;
string privateKey =”{Your Gluwacoin Private Key}”;
uint limit = “{Number of transactions to include in the result}”; // defaults to 100. Optional
ETransactionStatusFilter status = “{Incomplete or Confirmed}”; //defaults to Confirmed. Optional
uint offset = “{Number of transactions to skip}”; //default to 0. Optional”

Result<List<SuccessResponse>, ErrorResponse> result =await gluwaClient.GetTransactionListAsync(
    currency,
    address,
    privateKey,
    limit, // optional, default = 100
    status, // optional, default = Confirmed,
    offset // optional, default = 0
);

if (result.IsFailure)
{
    switch (result.Error.Code)
    {
        case "ErrorCode1":
            // handle error 1
            break;
        case "ErrorCode2":
            // handle error 2
            break;
        default:
            // handle error
            break;
    }
}
else
{
    // successful response. See result.Data for the response
}
```

#### [Retrieve Transaction Details by Hash](../api/api.md#retrieve-transaction-details-by-hash)

```csharp
ECurrency currency = “{USDG or KRWG}”;
string privateKey =”{Your Gluwacoin Private Key}” ;
string txnHash = “{Hash of the transaction on the blockchain}”;

Result<SuccessResponse, ErrorResponse> result =await gluwaClient.GetTransactionDetailsAsync(
currency,
privateKey,
txnHash
);

if (result.IsFailure)
{
    switch (result.Error.Code)
    {
        case "ErrorCode1":
            // handle error 1
            break;
        case "ErrorCode2":
            // handle error 2
            break;
        default:
            // handle error
            break;
    }
}
else
{
    // successful response. See result.Data for the response
}
```

#### [Retrieve a Balance for an Address](../api/api.md#retrieve-a-balance-for-an-address)

```csharp
ECurrency currency = “{USDG or KRWG}”;
string address = “{Your Gluwacoin public Address}”;

Result<SuccessResponse, ErrorResponse> result = await gluwaClient.GetBalanceAsync(
    currency, 
    address
);

if (result.IsFailure)
{
    switch (result.Error.Code)
    {
        case "ErrorCode1":
            // handle error 1
            break;
        case "ErrorCode2":
            // handle error 2
            break;
        default:
            // handle error
            break;
    }
}
else
{
    // successful response. See result.Data for the response
}
```

### QRCodeClient

#### [Create a Payment QR Code](../api/api.md#create-a-payment-qr-code)

```csharp
string apiKey =”{Your API Key}”;
string secret =”{Your API Secret}”;
string address = “{Your public address}”;
string privateKey =”{Your private Key}”; 
EPaymentCurrency currency = “{USDG or KRWG}”;
string amount =”{Payment amount}”;
string format = “{Desired image format}”; // defaults to null. return base64 string. Optional.
// if you want to receive an image file put ‘image/jpeg’ or ‘image/png’ instead.
string note = “{Additional information, used by the merchant user}”; //default to null. Optional
string merchantOrderID = “{Identifier for the payment, used by the merchant user}”; //default to null. Optional
int expiry = “{Time of expiry for the QR code in seconds}”; //default to 1800. Optional”

Result<string, ErrorResponse> result = await qRCodeClient.GetPaymentQRCodeAsync(
    apiKey,
    secret,
    address,
    privateKey,
    currency,
    amount,
    format, //optional, default = null
    note, //optional, default = null
    merchantOrderID, //optional, default = null
    expiry //optional, default = 1800
);

if (result.IsFailure)
{
    switch (result.Error.Code)
    {
        case "ErrorCode1":
            // handle error 1
            break;
        case "ErrorCode2":
            // handle error 2
            break;
        default:
            // handle error
            break;
    }
}
else
{
    // successful response. See result.Data for the response
}
```

### Webhook Validation

A method for validating if the webhook is from Gluwa or not. Gluwa sends a webhook request when a transaction is created or completed. Learn more about webhooks from Gluwa [here](../get-started/dashboard/webhooks.md).

```csharp
PayLoad payLoad = “{Payload}”;
PayLoad payLoad = new PayLoad()
{
    MerchantOrderID = “{Identifier for the transaction that was provided by the merchant user}”,
    EventType = {TransactionConfirmed or TransactionCreated or TransactionFailed},
    Type = {Webhook or PushNotification},
    ResourceID = "{Transcation ID}"
};
string signature = “{The value of X-REQUEST-SIGNATURE}”;
string webhookSecretKey = “{Your Webhook Secret}”;

Webhook.ValidateWebhook(
payLoad, 
signature,
webhookSecretKey);
```

The method returns `true` if the validation was successful.

