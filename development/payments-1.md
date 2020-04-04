---
description: >-
  Explore ways to integrate Gluwa QR code payments in your website or mobile
  app.
---

# Payment QR Codes

Collecting payments on Gluwa consists of creating a payment [QR code](https://en.wikipedia.org/wiki/QR_code) and receiving payment [webhook](https://en.wikipedia.org/wiki/Webhook) notifications. Gluwa users can scan the QR code with the Gluwa mobile application to make the payment. You can optionally sign up for a webhook notification to get notified when you receive payments.

This guide shows you how to include Gluwa QR codes on your checkout pages.

## Creating Payment QR Codes

To create a new QR code, submit an API request to the [Create Payment QR Code endpoint](../api/api.md#create-a-payment-qr-code) with the API key and secret.

{% page-ref page="../get-started/dashboard/api-keys.md" %}

You will need to generate a new QR code each time you need to receive a payment from a user.

### QR Code Example

After successfully submitting your API request, you will get a QR code image file as such:

![Payment QR Code Example](../.gitbook/assets/image%20%281%29.png)

## Display Payment QR Code

Now, display the QR code to your customers and ask for a payment. They can simply scan the QR code to make the payment.

{% page-ref page="../get-started/gluwa/make-qr-code-payments-1.md" %}



