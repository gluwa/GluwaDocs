---
description: Use webhooks to be notified about events that happen in a Gluwa account.
---

# Webhooks

You can receive a webhook notification from Gluwa when there is a transaction associated with your account. Note that Gluwa only notifies you if the transaction or an exchange was created by using Gluwa API.

To integrate Gluwa webhook to your service you need to:

1. Create a webhook endpoint and on your server
2. Register your webhook endpoint
3. Verify your address

## Step 1: Create a Webhook Endpoint

Create an endpoint that would accept the webhook requests and process them according to your business logic.

Webhook is sent as a POST request to the URL you will register in [Step 2](../get-started/dashboard/webhooks.md#register-webhook-endpoint). The webhook may or may not have a request body depending on the type of the event. For more information, see [Supported Webhook Events](webhooks.md#supported-webhook-events).

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
4. Exchange Success
5. Exchange Failed

{% hint style="info" %}
If you registered your webhook endpoint prior to XXXXX, see [V1 webhook](webhooks.md#version-1-v-1-webhook-events). Else, see [V2 webhook](webhooks.md#version-2-v-2-webhook-events).
{% endhint %}

### Version 1 \(V1\) Webhook Events

#### Common Fields For All Webhook Requests

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
      <td style="text-align:left">EventType</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>The event type
          <br />
        </p>
        <p><code>TransactionConfirmed</code>, <code>TransactionCreated</code>, <code>TransactionFailed</code>, <code>ExchangeSuccess</code>, <code>ExchangeFailed</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Type</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Always &quot;Webhook&quot;</td>
    </tr>
    <tr>
      <td style="text-align:left">ResourceID</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>The ID of the resource.
          <br />
        </p>
        <p>For <code>TransactionConfirmed</code>, <code>TransactionCreated</code>, <code>TransactionFailed</code>,
          this is the transaction hash.</p>
        <p>For <code>ExchangeSuccess</code>, <code>ExchangeFailed</code>, this is the
          order ID</p>
      </td>
    </tr>
  </tbody>
</table>

#### Transactions Related Events Only

| Attribute | Type | Description |
| :--- | :--- | :--- |
| MerchantOrderID | `string` | The merchant order ID |
| Amount | `string` | The amount that was received. |

#### Exchanges Related Events Only

| Attribute | Type | Description |
| :--- | :--- | :--- |
| OrderAmountRemaining | `string` | Amount remaining in the order |
| Conversion | `string` | Conversion symbol for the order. See [conversion](../api/currency-and-conversion-symbols.md#conversion-symbols).  |
| SendingAddress | `string` | The address where the source amount was sent from. |
| ReceivingAddress | `string` | The address where the exchanged amount was received. |
| SourceAmount | `string` | The amount that was sent in source currency. |
| Price | `string` | The price used for the exchange. The unit is `<exchanged currency>/<source currency>`. |
| ExchangedAmount | `string` | The amount that was received in exchanged currency. |

### Version 2 \(V2\) Webhook Events

#### Common Fields For All Webhook Requests

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
      <td style="text-align:left">ID</td>
      <td style="text-align:left"><code>UUID</code>
      </td>
      <td style="text-align:left">The ID of the webhook.</td>
    </tr>
    <tr>
      <td style="text-align:left">CreatedDateTime</td>
      <td style="text-align:left"><code>datetime</code>
      </td>
      <td style="text-align:left">The created date and time of the webhook.</td>
    </tr>
    <tr>
      <td style="text-align:left">ResourceType</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>The type of the resource.</p>
        <p></p>
        <p><code>Transaction</code>, <code>Exchange</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">EventName</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>The name of the event.</p>
        <p></p>
        <p><code>TRANSACTION.CONFIRMED</code>
        </p>
        <p><code>TRANSACTION.CREATED</code>
        </p>
        <p><code>EXCHANGE.SUCCESS</code>
        </p>
        <p><code>EXCHANGE.FAILED</code>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Summary</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The summary of the webhook.</td>
    </tr>
    <tr>
      <td style="text-align:left">Resource</td>
      <td style="text-align:left"><code>obj</code>
      </td>
      <td style="text-align:left">
        <p>The resource associated with the webhook. This can be either:</p>
        <ol>
          <li><a href="webhooks.md#transaction">Transaction</a>
          </li>
          <li><a href="webhooks.md#exchange">Exchange</a>
          </li>
        </ol>
      </td>
    </tr>
  </tbody>
</table>

#### Transaction

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
      <td style="text-align:left">ID</td>
      <td style="text-align:left"><code>UUID</code>
      </td>
      <td style="text-align:left">Gluwa&apos;s internal transaction ID.</td>
    </tr>
    <tr>
      <td style="text-align:left">TxHash</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The created date and time of the webhook.</td>
    </tr>
    <tr>
      <td style="text-align:left">Source</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The address of the sender.</td>
    </tr>
    <tr>
      <td style="text-align:left">Target</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The address of the receiver.</td>
    </tr>
    <tr>
      <td style="text-align:left">Amount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Transaction amount, not including the fee. This is the amount that the
        receiver receives.</td>
    </tr>
    <tr>
      <td style="text-align:left">Fee</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The fee amount.</td>
    </tr>
    <tr>
      <td style="text-align:left">Currency</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>The <a href="../api/currency-and-conversion-symbols.md#currency-symbols">currency</a> of
          the transaction.</p>
        <p></p>
        <p>Webhook does not support <code>BTC</code> Transactions.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Status</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">
        <p>The status of the transaction.<code>Unconfirmed</code>, <code>Confirmed</code>,
          or <code>Failed</code>.
          <br />
        </p>
        <p>Unconfirmed - The transaction was announce to the blockchain, but is not
          included in any block yet.
          <br />Confirmed - The transaction was included in the blockchain and received
          a confirmation. Failed - The transaction has failed for some reason.</p>
      </td>
    </tr>
  </tbody>
</table>

#### Exchange

| Attribute | Type | Description |
| :--- | :--- | :--- |
| ID | `UUID` | The ID of the order. |
| OrderAmountRemaining | `string` | Amount remaining in the order. |
| Conversion | `string` | Conversion symbol for the order. See [conversion](../api/currency-and-conversion-symbols.md#conversion-symbols).  |
| SendingAddress | `string` | The address where the source amount was sent from. |
| ReceivingAddress | `string` | The address where the exchanged amount was received. |
| SourceAmount | `string` | The amount that was sent in source currency. |
| Price | `string` | The price used for the exchange. The unit is `<exchanged currency>/<source currency>`. |
| ExchangedAmount | `string` | The amount that was received in exchanged currency. |

