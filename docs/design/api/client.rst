.. _client-label:

Client API
====================

The selling site will need to include the `payments-client library <https://github.com/mozilla/payments-client>`_.
The client is available on `npm <https://www.npmjs.com/package/mozilla-payments-client>`_::

    npm install mozilla-payments-client

Then add in:

.. code-block:: javascript

        var client = new window.PaymentsClient({
          accessToken: firefox_accounts_access_token,
          product: {
              id: 'product-id-from-payments-config',
              image: 'http://url.to.product.image'
          },
        });

Parameters:

* `httpsOnly`: if `true`, the payments-ui will check to see if requests are using HTTPS. Should be set to `true` for production. Intended for development where HTTPS might not be available. Defaults to `true`.
* `accessToken`: an access token returned by Firefox Accounts.
* `product.id`: the id of the product from :ref:`payments config <configuration-label>`
* `product.image`: URL of an image to be shown in the purchase flow
* `paymentHost`: (optional) the server used to process payments [*]_

.. [*] This will default to the production server when it exists. Currently its going to default to a local Docker development instance: `http://pay.dev:8000`
