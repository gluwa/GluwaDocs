# Exchange Webhook

The Exchange Request Webhook is a special webhook that allows you to receive an exchange request on the order you've created. This is a different webhook from the webhook you use to receive on various events like transaction confirmed, transaction created and transaction failed. As a market provider, you must register one Exchange Request Webhook URL to create order and accept any exchange request.

## Create an **Exchange Request Webhook URL** 

Go to the dashboard and register a webhook URL to receive exchange request through this URL. Once you register a webhook URL, there will be a checkbox beside the URL. Check this box to mark the URL as the exchange request webhook URL.

![](.gitbook/assets/image%20%282%29.png)

{% hint style="warning" %}
If you unset this checkbox, this will immediately suspend any exchange request webhooks, which means that you will not be able to accept any exchange requests until you recheck this box.
{% endhint %}

## **The Exchange Request**

When an exchange is requested using your order, Gluwa will make a POST request to the exchange webhook URL for the permission to proceed with the exchange. The following request body is sent with the request.

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
      <td style="text-align:left">ID of the exchange request</td>
    </tr>
    <tr>
      <td style="text-align:left">EventType</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Always &quot;ExchangeRequest&quot;</td>
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
      <td style="text-align:left">Order ID</td>
    </tr>
    <tr>
      <td style="text-align:left">Conversion</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The <a href="api/currency-and-conversion-symbols.md#conversion-symbols">conversion</a> of
        the exchange.</td>
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
      <td style="text-align:left">Executor</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left"><em><b>Optional.</b></em> Required if the source currency is Gluwacoin
        currency (ex&gt; <code>USD-G</code>, <code>KRW-G</code>). The address that
        will execute the exchange on your behalf. You need this to sign the reserve
        transaction signature.</td>
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
</table>

{% hint style="info" %}
If you want to verify that the request was actually sent from Gluwa, you can check the veracity of the webhook as shown [here](development/webhooks.md#checking-the-veracity-of-a-request-using-x-request-signature).
{% endhint %}

After the you receive the webhook receive, you must do the following to accept the exchange request:

1. Return 200 status code as a response to the webhook request. If you return any other response, Gluwa will try to send the same webhook for the maximum of 5 times, before treating the exchange request as declined.

2. Use [PATCH /V1/ExchangeRequest/{ID}](exchange-request.md#patch-v-1-exchangerequests-id) endpoint to accept the exchange request. If you fail to successfully accept the exchange request within 10 minutes, the exchange request is automatically declined.

If you decline the exchange request for an order 5 times, that order will be canceled automatically.

## **Receiving Exchange Success and Failed Webhooks**

Gluwa will send a webhook when an exchange succeeds or fails. This will be sent to the same webhook URL as the one you use to receive transaction confirmed, transaction created and transaction failed webhooks, **NOT** to the exchange equest webhook URL.

### **ExchangeSuccess Event**

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
      <td style="text-align:left">Always &quot;ExchangeSuccess&quot;</td>
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
      <td style="text-align:left">Order ID</td>
    </tr>
    <tr>
      <td style="text-align:left">OrderAmountRemaining</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The amount remaining in the order.</td>
    </tr>
    <tr>
      <td style="text-align:left">Conversion</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Conversion symbol for the order. See <a href="api/currency-and-conversion-symbols.md#conversion-symbols">conversion</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left">SendingAddress</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The address that will fund the source amount.</td>
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
      <td style="text-align:left">SourceAmount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The amount in source currency that this order will exchange.</td>
    </tr>
    <tr>
      <td style="text-align:left">Price</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The price the order will use for the exchange. The unit is <code>&lt;exchanged currency&gt;/&lt;source currency&gt;</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">ExchangedAmount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The exchanged amount in exchanged currency.</td>
    </tr>
  </tbody>
</table>

### **ExchangeFailed Event**

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
      <td style="text-align:left">Always &quot;ExchangeFailed&quot;</td>
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
      <td style="text-align:left">Order ID</td>
    </tr>
    <tr>
      <td style="text-align:left">OrderAmountRemaining</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The amount remaining in the order.</td>
    </tr>
    <tr>
      <td style="text-align:left">Conversion</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">Conversion symbol for the order. See <a href="api/currency-and-conversion-symbols.md#conversion-symbols">conversion</a>.</td>
    </tr>
    <tr>
      <td style="text-align:left">SendingAddress</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The address that will fund the source amount.</td>
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
      <td style="text-align:left">SourceAmount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The amount in source currency that this order will exchange.</td>
    </tr>
    <tr>
      <td style="text-align:left">Price</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The price the order will use for the exchange. The unit is <code>&lt;exchanged currency&gt;/&lt;source currency&gt;</code>.</td>
    </tr>
    <tr>
      <td style="text-align:left">ExchangedAmount</td>
      <td style="text-align:left"><code>string</code>
      </td>
      <td style="text-align:left">The exchanged amount in exchanged currency.</td>
    </tr>
  </tbody>
</table>

