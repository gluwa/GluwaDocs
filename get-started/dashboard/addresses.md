---
description: Register your address and get webhook notifications
---

# Addresses

## Register Account Address

Gluwa will listen to registered addresses and wait for transaction confirmation events. Open [Gluwa Dashboard Registered Addresses](https://dashboard.gluwa.com/addresses) page to register your addresses.

![Register New Address Page](../../.gitbook/assets/register-address.png)

Note that the form requires you to sign an arbitrary message with the private key of the address. The signature will prove your ownership of the private key.

You can create the signature on Gluwa mobile application.

{% page-ref page="../gluwa/create-a-signature.md" %}

![Gluwa Dashboard Registered Addresses Page](../../.gitbook/assets/screen-shot-2019-09-02-at-1.08.33-am.png)

Once you register your address, Gluwa will send a webhook to your registered webhook URL each time the address has a newly confirmed transaction.

