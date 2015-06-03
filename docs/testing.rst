Testing
=======

When you are using the sandbox, Braintree has a `comprehensive suite of cards and
conditions <https://developers.braintreepayments.com/javascript+python/reference/general/testing>`_
that can cause failures. To trigger some of these conditions you will need
access to the Braintree sandbox for the server in question.

For all the following errors, the error documentation at Braintree is definitive.

Example error
-------------

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

        {"braintree": {"__all__": [{"message": "Invalid Secure Payment Data", "code": "2078"}]}

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
