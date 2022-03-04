# Idempotent Requests

When creating a transaction, the API supports [idempotency](https://en.wikipedia.org/wiki/Idempotence) for safely retrying requests without accidentally processing the same operation twice. In case your request is disrupted, and you do not receive a response, you can retry the same request using the same idempotency key to guarantee that you don't create two identical transactions.

To make an idempotent transaction request, provide a value for the `Idem` attribute in the [Transaction](https://app.gitbook.com/@gluwa/s/gluwa-documentation/\~/drafts/-M4pEZ8L7EwSLFdquoYR/api/transaction#request-body) request body. The `Idem` value must be `UUID` to guarantee the uniqueness of the transaction. If Gluwa already received a request with the same Idem value and the transaction has begun processing, any subsequent requests that use the same Idem value will be denied with 409 response regardless of whether the first transaction succeeded or failed.
