---
description: Whitelist your wallet address and get webhook notifications
---

# Addresses

This guide assumes that you've setup your webhook endpoints already. If you haven't, follow the Webhook guide below first.

{% content-ref url="webhooks.md" %}
[webhooks.md](webhooks.md)
{% endcontent-ref %}

## Register Your Wallet Address

Gluwa will listen to any event that occurs to registered addresses and send webhook notifications to the registered webhook URLs. Open [Gluwa Dashboard Registered Addresses](https://dashboard.gluwa.com/addresses) page to register your addresses.

![Register New Address Page](../../.gitbook/assets/register-address.png)

Note that the form requires you to sign an arbitrary message with the private key of the address. The signature will prove your ownership of the address.

You can create the signature on Gluwa mobile application, or using any other third party apps like below:

1. MyEtherWallet (Ethereum)
2. MetaMask (Ethereum)
3. Electrum (Bitcoin)

{% hint style="info" %}
The above list of third party apps is not a comprehensive list. There are many other apps that provide message signing functionality, so you can use any of them.
{% endhint %}

{% content-ref url="../gluwa/create-a-signature.md" %}
[create-a-signature.md](../gluwa/create-a-signature.md)
{% endcontent-ref %}

![Gluwa Dashboard Registered Addresses Page](../../.gitbook/assets/screen-shot-2019-09-02-at-1.08.33-am.png)

Once you register your address, Gluwa will send a webhook to your registered webhook URL each time any of the supported events occur on your address. You can find the supported events [here](webhooks.md#supported-events).
