---
description: Learn how to use various endpoints of the Gluwa API.
---

# API Reference

## Introduction

The Gluwa API follows [REST](http://en.wikipedia.org/wiki/Representational_State_Transfer) design guideline. The API has predictable resource-oriented URL's and returns [JSON-encoded](http://www.json.org/) responses, and uses standard HTTP response codes, authentication, and verbs.

We support a separate environment for testing purposes. When testing, use the sandbox credentials and URL. When you are ready to go live, use the production credentials and URL.

{% page-ref page="../development/environments.md" %}

### Base URL

{% tabs %}
{% tab title="Live" %}
```http
https://api.gluwa.com
```
{% endtab %}

{% tab title="Sandbox" %}
```http
https://sandbox.api.gluwa.com
```
{% endtab %}
{% endtabs %}

### HTTPS Over HTTP

All API requests must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTP_Secure). Calls made over plain HTTP will fail. 

### Client SDK's

By default, the Gluwa API Docs demonstrate using curl to interact with the API over HTTP. You could also use one of our official Software Development Kits \(SDK\) to see interact with our API.

{% page-ref page="../development/gluwa-sdk-for-php.md" %}

{% page-ref page="../development/gluwa-sdk-for-.net.md" %}

## Authorization

The Gluwa API utilizes various authorization methods for each endpoint. Follow the link below to learn more.

{% page-ref page="authentication.md" %}

## Core Resources

{% page-ref page="balance.md" %}

{% page-ref page="fee.md" %}

{% page-ref page="transaction.md" %}

{% page-ref page="qr-code.md" %}

{% page-ref page="../quote.md" %}

{% page-ref page="../order.md" %}

{% page-ref page="../order-book.md" %}

