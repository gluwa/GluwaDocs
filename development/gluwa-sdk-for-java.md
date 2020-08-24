# Gluwa SDK for JAVA

If your service is developed in JAVA, the features we provide are available through the SDK. The Gluwa SDK for JAVA is a library with powerful features that enable JAVA developers to easily make requests to the Gluwa APIs.

## Getting started

```markup
<repositories>
    <repository>
        <id>Gluwa-java-mvn-repo</id>
        <url>https://raw.github.com/gluwa/Gluwa-Java/mvn-repo/</url>
        <snapshots>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
        </snapshots>
    </repository>
</repositories>

<dependencies>
    <dependency>
        <groupId>com.gluwa.sdk</groupId>
        <artifactId>gluwa-sdk-java</artifactId>
        <version>1.0.0</version>
    </dependency>
</dependencies>
```

Our jar is here: [https://github.com/gluwa/Gluwa-Java/tree/mvn-repo/com/gluwa/sdk/gluwa-sdk-java](https://github.com/gluwa/Gluwa-Java/tree/mvn-repo/com/gluwa/sdk/gluwa-sdk-java)

Create and initialize a `Configuration` class. Then, enter the `APIKey`, `APISecret` and `WebookSecret` generated from the [Gluwa Dashboard](https://dashboard.gluwa.com), and an Ethereum wallet to manage your funds. You can use credentials from the sandbox dashboard and a Rinkeby wallet if you want to test in the sandbox environment.

```java
public class Configuration extends Configuration {
	public ConfigurationForTest() {
		super();
		set__DEV__(true); // sandbox: true, live: false
		setApiKey("{Your API Key}");
		setApiSecret("{Your API Secret}");
		setWebhookSecret("{Your Webhook Secret}");
		setMasterEthereumAddress("{Your Ethereum Address}");
		setMasterEthereumPrivateKey("{Your Ethereum Private Key}");
	}
}
```

{% hint style="warning" %}
Gluwa SDK For JAVA works with JDK 1.8 or higher.
{% endhint %}

## Method Examples <a id="method-examples"></a>

#### ​[Create a New Transaction](https://app.gitbook.com/@gluwa/s/gluwa-documentation/~/drafts/-MEwcvFqT7xZLjp3bwpG/api/api#create-a-new-transaction)​ <a id="create-a-new-transaction"></a>

```java
public void postTransaction_test() {
	GluwaApiSDKImpl wrapper = new GluwaApiSDKImpl(new ConfigurationForTest());
	GluwaTransaction transaction = new GluwaTransaction();

	transaction.setCurrency("{Currency}"); // Currency.KRWG, Currency.NGNG
	transaction.setAmount("{Amount}");
	transaction.setTargetAddress("{Target's Address}");
	transaction.setNote("{Note}");
	transaction.setMerchantOrderID("{Your OrderID}");

	GluwaResponse result = wrapper.postTransaction(transaction);
}
```

#### ​[Create a Payment QR Code](https://app.gitbook.com/@gluwa/s/gluwa-documentation/~/drafts/-MEwcvFqT7xZLjp3bwpG/api/api#create-a-payment-qr-code)​ <a id="create-a-payment-qr-code"></a>

`getPaymentQRCode` API returns a QR code png image as a Base64 string. You can display the image on your website as below:

```markup
<img src="data:image/png;base64,{BASE64_STRING_YOU_RECEIVED}" alt="Gluwa Payment QR Code">
```

#### ​[List Transaction History for an Address](https://app.gitbook.com/@gluwa/s/gluwa-documentation/~/drafts/-MEwcvFqT7xZLjp3bwpG/api/api#list-transaction-history-for-an-address)​ <a id="list-transaction-history-for-an-address"></a>

```java
public void getListTransactionHistory_test() {
  Configuration conf = new ConfigurationForTest();
  GluwaApiSDKImpl wrapper = new GluwaApiSDKImpl(conf);

  GluwaTransaction transaction = new GluwaTransaction();
  transaction.setCurrency("{Currency}");
  transaction.setLimit(100); // optional
  transaction.setStatus("Confirmed"); // optional
  transaction.setOffset(0); // optional

  GluwaResponse result = wrapper.getListTransactionHistory(transaction);
}
```

#### ​[Retrieve Transaction Details by Hash](https://app.gitbook.com/@gluwa/s/gluwa-documentation/~/drafts/-MEwcvFqT7xZLjp3bwpG/api/api#retrieve-transaction-details-by-hash)​ <a id="retrieve-transaction-details-by-hash"></a>

```java
public void getListTransactionDetail_test() {
  Configuration conf = new ConfigurationForTest();
  GluwaApiSDKImpl wrapper = new GluwaApiSDKImpl(conf);

  GluwaTransaction transaction = new GluwaTransaction();
  transaction.setCurrency("{Currency}");
  transaction.setTxnHash("{Txn Hash}");

  GluwaResponse result = wrapper.getListTransactionDetail(transaction);
}
```

#### ​[Retrieve a Balance for an Address](https://app.gitbook.com/@gluwa/s/gluwa-documentation/~/drafts/-MEwcvFqT7xZLjp3bwpG/api/api#retrieve-a-balance-for-an-address)​ <a id="retrieve-a-balance-for-an-address"></a>

```java
public void getAddresses_Test() {
  Configuration conf = new ConfigurationForTest();
  GluwaApiSDKImpl wrapper = new GluwaApiSDKImpl(conf);

  GluwaTransaction transaction = new GluwaTransaction();
  transaction.setCurrency("{Currency}");

  GluwaResponse result = wrapper.getAddresses(transaction);
}
```

#### ​[Webhook Validation](https://app.gitbook.com/@gluwa/s/gluwa-documentation/~/drafts/-MEwcvFqT7xZLjp3bwpG/development/webhooks#step-3-verify-your-wallet-address)​ <a id="webhook-validation"></a>

When the user completes transfer via the QR code, the Gluwa API sends a webhook to your webhook endpoint. Verify that the values ​​actually sent by the Gluwa server are correct.‌

Verify the requested Signature and Payload as follows:

```java
public void validateWebhook_test() {
  Configuration conf = new ConfigurationForTest();
  GluwaApiSDKImpl wrapper = new GluwaApiSDKImpl(conf);
  boolean result = wrapper.validateWebhook(
    "{Payload in Request's body}",
    "{Signature in Request's header}");
}
```

