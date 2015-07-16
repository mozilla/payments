Testing
=======

When you are using the sandbox, Braintree has a `comprehensive suite of cards and
conditions <https://developers.braintreepayments.com/javascript+python/reference/general/testing>`_
that can cause failures. To trigger some of these conditions you will need
access to the Braintree sandbox for the server in question.

For all the following errors, the error documentation at Braintree is definitive.

Errors
------

An error returned by the API in `payments-service <http://payments-service.readthedocs.org/en/latest/topics/api.html#errors>`_:

    .. code-block:: json

        {
            "error_message": "Bad Request",
            "error_response": {
                "braintree": {
                    "__all__": [
                        {"message": "Invalid Secure Payment Data", "code": "unknown"}
                    ]
                }
            }
        }

General errors
--------------

These are generally fatal errors the user can't really do anything about. Here's how to trigger some different example errors.

Do not Honour
+++++++++++++

* Enter card 5105105105105100 and everything else as usual.

    .. code-block:: json

        {"braintree": {"__all__": [{"message": "Do Not Honor", "code": "2000"}]}}

Fraud
+++++

* Go to Braintree Sandbox and turn on fraud checking.
* Enter card 4000111111111511 and everything else as usual.

    .. code-block:: json

        {"braintree": {"__all__": [{"message": "Gateway Rejected: fraud", "code": "fraud"}]}}

Invalid Secure Payment Data
+++++++++++++++++++++++++++

* Go to the Braintree Sandbox and set the price of mozilla-concrete-brick to $2708.
* Enter card 4111111111111111 and everything else as usual.

    .. code-block:: json

        {"braintree": {"__all__": [{"message": "Invalid Secure Payment Data", "code": "2078"}]}}

Specific errors
---------------

These are usually errors that the user can do something about.

Bad CVV
+++++++

* Go to Braintree Sandbox and turn on CVV checking.
* Enter card 4111111111111111 and enter a CVV of 200.

    .. code-block:: json

        {"braintree": {"cvv": [{"message": "Gateway Rejected: cvv", "code": "cvv"}]}}

Bad Credit card number
++++++++++++++++++++++

* Enter a few characters into the card number.
* Open Firefox developer tools and remove disabled from the "Subscribe" field.
* Click "Subscribe"

    .. code-block:: json

        {
            "braintree": {
                "number": [
                    {"message": "Credit card type is not accepted by this merchant account.", "code": "81703"},
                    {"message": "Credit card number is invalid.", "code": "81715"}
                ]
            }
        }

No expiry
+++++++++

* Enter everything else except a date.
* Open Firefox developer tools and remove disabled from the "Subscribe" field.
* Click "Subscribe"

    .. code-block:: json

        {"braintree": {"expiration_date": [{"message": "Expiration date is required.", "code": "81709"}]}}

Expiry is invalid
+++++++++++++++++

* Enter everything else normally, but enter a silly date, eg: 99/99.
* Open Firefox developer tools and remove disabled from the "Subscribe" field.
* Click "Subscribe"

    .. code-block:: json

        {"braintree": {"expiration_date": [{"message": "Expiration date is invalid.", "code": "81710"}]}}

Generating webhooks
-------------------

A webhook is an event sent by Braintree to certain events.

.. note:: If you are in development and reseting your database,
          subscriptions and webhooks in Braintree may not be in sync
          with your local database. Causing webhooks that are no longer
          relevant to be sent to your local servers.

Configuring braintree
+++++++++++++++++++++

When you are using the sandbox go to Settings > Webhooks. Add in your server,
for example::

    http://pay.dev.mozaws.net:8000/api/braintree/webhook/

You can select all "notifications to send" or just pick the ones we actually
process which are: Subscription Canceled, Subscription Charged Successfully
and Subscription Charged Unsuccessfully.

If you are doing local development, you might need to expose your local server
publicly. Something like `ngrok <http://ngrok.com>`_ can do this easily by
entering::

    ngrok pay.dev:8000

Then enter the corresponding URL into the Braintree sandbox.

Sending webhooks manually
+++++++++++++++++++++++++

We have a tool to send webhook requests from the command line. This is a
development tool and is not designed to be a complete replacement for proper
end-to-end testing with Braintree. In all cases, Braintree is right and this
tool is wrong.

.. note:: To use this tool, the solitude container needs to make a request to the
          payments-service server, usually this means http://pay.dev:8000/, you might need
          to use an IP address if that doesn't resolve.

Inside the solitude container run::

    python manage.py braintree_webhook --parse=subscription_charged_successfully --subscription_id=latest

This will generate and send a `subscription_charged_successfully` webhook to
the payments-service for the last subscription added to the database.

See the code in `solitude <https://github.com/mozilla/solitude/blob/master/lib/brains/management/commands/braintree_webhook.py>`_
 for more options.

subscription_charged_successfully
+++++++++++++++++++++++++++++++++

To generate this, simply complete a purchase within payments. We'll immediately
try to create subscription and charge the credit card. If that completes then
his webhook will be sent.

This can also be run from the command line.

subscription_charged_unsuccessfully
+++++++++++++++++++++++++++++++++++

To generate this, go to the Braintree sandbox and alter the plan you are testing
with to cost $2708 and have a one day trial period. Purchase the product. Then
in one day you will get a webhook.

This can also be run from the command line, without delay.

subscription_canceled
+++++++++++++++++++++++++++++++++

To generate this, go to the Braintree sandbox, select Subscriptions, search for
the subscription you'd like to cancel and then click "Cancel".

This can also be run from the command line.
