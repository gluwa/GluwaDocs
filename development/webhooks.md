---
description: Use webhooks to be notified about events that happen in a Gluwa account.
---

# Webhooks

You can receive a webhook notification from Gluwa when there is a transaction associated with your account. Note that Gluwa only notifies you if the transaction was created by Gluwa API.

To integrate Gluwa webhook to your service you need to:

1. Create a webhook endpoint and on your server
2. Register your webhook endpoint
3. Verify your address

## Step 1: Create a Webhook Endpoint

Create an endpoint that would accept the webhook requests and process them according to your business logic.

Webhook is sent as a POST request to the URL you will register in [Step 2](../get-started/dashboard/webhooks.md#register-webhook-endpoint). The webhook may or may not have a request body depending on the type of the event. For example, transaction confirmed event will have the following request body.

{% code title="Example Webhook Notification" %}
```javascript
{
    "Data": {
        "EventType": "TransactionConfirmed",
        "Type": "Webhook",
        "ResourceID": "{ID of the resource. In this case, this will be transaction hash}",
        "MerchantOrderID": "{Merchant OrderID if you specified MerchantOrderID field when generating QR code}"
        "Amount": "{Amount received}"
    }
}
```
{% endcode %}

### Checking the veracity of a request using X-REQUEST-SIGNATURE

To verify that the webhook is actually sent by Gluwa, you must check the validity of`X-REQUEST-SIGNATURE` header. This involves generating `X-REQUEST-SIGNATURE` value on your own server and comparing it against the value of the `X-REQUEST-SIGNATURE` header sent with the webhook.

To generate `X-REQUEST-SIGNATURE`:

1. Get your webhook secret from the [Gluwa Dashboard Webhooks](https://dashboard.gluwa.com/Webhook) page 
2. Get the request body you received
3. Run the following formula:

{% code title="Generating X-REQUEST-SIGNATURE for a Webhook" %}
```csharp
Base64UrlSafeEncode(HMACSha256({request body in JSON string}, {webhook secret}))
```
{% endcode %}

{% hint style="warning" %}
The request body JSON string must be in minified format, meaning no spaces and line breaks between JSON keys and values.
{% endhint %}

Compare the resulting value with the `X-REQUEST-SIGNATURE` header from the webhook. If identical, you have successfully verified the signature.

## Step 2: Register your webhook endpoint

The webhook will be sent to URLs that are registered through [Gluwa Dashboard](https://dashboard.gluwa.com). Visit the page below to learn how to register your webhook endpoint.

You may use online webhook testing tools such as [Webhook.site](https://webhook.site/) to test the webhook.

{% page-ref page="../get-started/dashboard/webhooks.md" %}

## Step 3: Whitelist your wallet address

You will receive webhooks only for the events involving whitelisted wallet addresses. Visit the page below to learn how to whitelist your wallet address.

{% page-ref page="../get-started/dashboard/addresses.md" %}

## Supported Webhook Events

Currently, we support webhooks for the following events:

1. Transaction Confirmed
2. Transaction Created
3. Transaction Failed

