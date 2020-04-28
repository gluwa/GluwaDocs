---
description: Set up webhook to your wallet and get notified
---

# Webhooks

You can receive webhook notifications from Gluwa whenever an event associated with your Gluwacoin or Bitcoin wallet is triggered.

## Get Webhook Secret

1. Visit the Webhook Management page at [https://dashboard.gluwa.com/webhook](https://dashboard.gluwa.com/webhook).
2. You will see an auto-generated Webhook Secret of your account on the page.
3. Your Webhook Secret is covered in grey boxes by default for your security. Click on the eye-shaped icon to reveal your Webhook Secret.

![Gluwa Dashboard Webhooks Page](../../.gitbook/assets/screen-shot-2019-09-02-at-12.42.18-am.png)

{% hint style="warning" %}
**Sandbox API keys and Production API keys are different.** This ensures that you don't modify your live customers data or charge them accidentally. Refer to Environments to learn how to use the sandbox mode.
{% endhint %}

## Register Webhook Endpoint

1. Visit the Webhook Management page at [https://dashboard.gluwa.com/webhook](https://dashboard.gluwa.com/webhook).
2. Click "REGISTER NEW URL" button.
3. Register your webhook endpoints.

![Webhook Endpoint Registration Page](../../.gitbook/assets/screen-shot-2019-10-19-at-11.10.45-am.png)

After you've registered webhook endpoints, you must whitelist the wallet addresses to receive webhooks.

{% page-ref page="addresses.md" %}

