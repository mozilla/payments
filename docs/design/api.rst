Proposed backend API
====================

Payment API
-----------

This is a rough draft of the backend API for Payments for Firefox Accounts.
This is based on solitude, but at this point it might not need to be.

TODO: figure out the appropriate relationship between seller and product.

This is the API needed to have a buyer who has *never purchased* anything with
Mozilla before complete a subscription sign up.

* **POST /buyer**: create a buyer.
* **POST /provider/tokenized-payment-method**: A stub for getting tokenized
  payment data from the provider.
* **POST /payment**: create a payment method. Requires: buyer, payment
  method information.
* **POST /subscription**: create a subscription. Requires: buyer, seller and
  payment method.
* **POST /transaction**: create a solitude transaction to start the first
  subscription. Requires: subscription.
* **POST /provider/charge-subscription**: tries to run a charge against the
  subscription at the payment provider.
* **PATCH /transaction/int:id**: update the transaction with information from
  the charge attempt. Requires: subscription attempt information.

To create a new payment method:

* **POST /provider/tokenized-payment-method**: A stub for getting tokenized
  payment data from the provider.
* **POST /payment**: create a payment method. Requires: buyer, payment
  information.

To change a subscription method:

* **PATCH /subscription/int:id**: updates the subscription. Requires: new
  payment method information. The buyer and seller could not be changed.

To cancel a subscription:

* **PATCH /subscription/int:id**: patch a subscription. Requires: new state
  of subscription active/inactive.

To get a list of all transactions:

* **GET /transaction?buyer=...**: filter buy buyer. This would be paginated, so
  we should order by newest first.

To get a list of all subscriptions:

* **GET /subscription?buyer=...**: filter buy buyer. This would be paginated,
  so we should order by newest first.

Provider API
------------

TODO: depends upon the provider we choose.

Events
------

The backend will need to send the following events, these might be sent as
emails, or other method, or ignored.

* Payment method created.
* Payment method updated for subscription.
* Subscription created.
* Subscription charge succeeded (e.g. every month).
* Subscription charge failed.
* Subscription cancelled.

Event listeners
---------------

TODO: would this go out through a queue.
