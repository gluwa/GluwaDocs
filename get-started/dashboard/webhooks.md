---
description: Set up webhook to your wallet and notified
---

# Webhooks

You can receive a webhook notification from Gluwa whenever a transaction associated with your registered Gluwacoin wallet is confirmed on the blockchain.

## Get Webhook Secret

1. Visit the Webhook Management page at [https://dashboard.gluwa.com/webhook](https://dashboard.gluwa.com/webhook).
2. You will see an auto-generated Webhook Secret of your account on the page.
3. Your Webhook Secret is covered in grey boxes by default for your security. Click on the eye-shaped icon to reveal your Webhook Secret.

![Gluwa Dashboard Webhooks Page](../../.gitbook/assets/screen-shot-2019-09-02-at-12.42.18-am.png)

## Register Webhook Endpoint

1. Visit the Webhook Management page at [https://dashboard.gluwa.com/webhook](https://dashboard.gluwa.com/webhook).
2. Register your webhook endpoints on the webhook management page.
3. Gluwa sends a webhook to the registered URLs.

![Webhook Endpoint Registration Page](../../.gitbook/assets/screen-shot-2019-10-19-at-11.10.45-am.png)

{% hint style="warning" %}
**Use only your sandbox webhook secret for testing and development**. This ensures that you don't accidentally modify your live customers or charges. Refer to [Environments](../../development/environments.md#sandbox-environment-urls) to learn how to use the sandbox mode.
{% endhint %}

