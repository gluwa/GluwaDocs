---
description: Learn how to use various endpoints of the Gluwa API.
---

# API Reference

## Introduction

The Gluwa API follows [REST](http://en.wikipedia.org/wiki/Representational\_State\_Transfer) design guideline. The API has predictable resource-oriented URL's and returns [JSON-encoded](http://www.json.org) responses, and uses standard HTTP response codes, authentication, and verbs.

We support a separate environment for testing purposes. When testing, use the sandbox credentials and URL. When you are ready to go live, use the production credentials and URL.

{% content-ref url="../development/environments.md" %}
[environments.md](../development/environments.md)
{% endcontent-ref %}

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

All API requests must be made over [HTTPS](http://en.wikipedia.org/wiki/HTTP\_Secure). Calls made over plain HTTP will fail.&#x20;

### Client SDK's

By default, the Gluwa API Docs demonstrate using curl to interact with the API over HTTP. You could also use one of our official Software Development Kits (SDK) to see interact with our API.

{% content-ref url="../development/gluwa-sdk-for-php.md" %}
[gluwa-sdk-for-php.md](../development/gluwa-sdk-for-php.md)
{% endcontent-ref %}

{% content-ref url="../development/gluwa-sdk-for-.net.md" %}
[gluwa-sdk-for-.net.md](../development/gluwa-sdk-for-.net.md)
{% endcontent-ref %}

## Authorization

The Gluwa API utilizes various authorization methods for each endpoint. Follow the link below to learn more.

{% content-ref url="authentication.md" %}
[authentication.md](authentication.md)
{% endcontent-ref %}

## Core Resources

{% content-ref url="balance.md" %}
[balance.md](balance.md)
{% endcontent-ref %}

{% content-ref url="fee.md" %}
[fee.md](fee.md)
{% endcontent-ref %}

{% content-ref url="transaction.md" %}
[transaction.md](transaction.md)
{% endcontent-ref %}

{% content-ref url="qr-code.md" %}
[qr-code.md](qr-code.md)
{% endcontent-ref %}

{% content-ref url="wrap-unwrap.md" %}
[wrap-unwrap.md](wrap-unwrap.md)
{% endcontent-ref %}

{% content-ref url="../quote.md" %}
[quote.md](../quote.md)
{% endcontent-ref %}

{% content-ref url="../order.md" %}
[order.md](../order.md)
{% endcontent-ref %}

{% content-ref url="../order-book.md" %}
[order-book.md](../order-book.md)
{% endcontent-ref %}

