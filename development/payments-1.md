---
description: >-
  Explore ways to integrate Gluwa QR code payments in your website or mobile
  app.
---

# QR Codes

Collecting payments on Gluwa consists of creating a payment [QR code](https://en.wikipedia.org/wiki/QR_code) and having the user scan the QR code with Gluwa mobile application to make a payment. You can optionally sign up for a webhook notification to get notified when you receive payments.

This guide shows you how to include Gluwa QR codes on your checkout pages.

## Types of QR Code

Gluwa has two types of QR code:

* **Transfer QR Code**: A user can generate Transfer QR code using Gluwa mobile application. If you scan the QR code using Gluwa mobile application, you initiate a transfer to the user. This QR code initiates an exact amount transfer. So you see the fee information on the transaction summary page.
* **Payment QR Code**: A user can generate Payment QR code using Gluwa API. The user needs to obtain API keys to do this. This QR code forces the receiver to pay the fee. This means that Gluwa mobile application does not show the fee information on the transaction summary page.

## Creating Payment QR Codes

To create a new QR code, submit an API request to the [Create Payment QR Code endpoint](../api/qr-code.md) with the API key and secret.

{% page-ref page="../get-started/dashboard/api-keys.md" %}

You will need to generate a new QR code each time you need to receive a payment from a user.

### QR Code Example

After successfully submitting your API request, you will get a QR code image file like below:

![Payment QR Code Example](../.gitbook/assets/image%20%281%29.png)

## Display Payment QR Code

Now, display the QR code to your customers and ask for a payment. They can simply scan the QR code to make the payment.

{% page-ref page="../get-started/gluwa/make-qr-code-payments-1.md" %}



