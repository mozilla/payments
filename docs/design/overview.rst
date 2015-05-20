Overview
--------

What is it?
===========

A Payments service for Mozilla that accepts payments for Mozilla properties or
services and directs the money to Mozilla.

Compared to Marketplace Payments
++++++++++++++++++++++++++++++++

This is quite different from Marketplace payments in a few key areas, namely:

* A Marketplace payment is from an end user to a developer, specifically not
  someone at Mozilla. This has very significant financial, legal and accounting
  requirements.

* The Marketplace integration with payment processors tends to require a lot of
  customization because Mozilla's technology enables a payment transaction
  between two external parties.

* In a Marketplace payment Mozilla gets a (really rather small) cut of the
  transaction at the end, which is sent from the payment provider.

* The Marketplace is designing a system for app and in-app payments that will
  need to be used and configured by many thousands of developers. The security
  of these apps is somewhat out of our control so our own threat model must
  account for malicious or compromised apps.

* The Marketplace payment service competes with every other web based payment system available.

* Marketplace payments are designed primarily for carrier billing on mobile.

* Just like the Marketplace service, Mozilla does not intend to ever directly
  process a payment (e.g. charge a credit card). That will be handed off to a
  payment provider like Stripe or Bango.

Do we need to use mozPay?
+++++++++++++++++++++++++

Nope. `mozPay <https://wiki.mozilla.org/WebAPI/WebPayment>`_ only exists on
Firefox OS and Android. Further, mozPay puts users straight into the
Marketplace flow because that's in the inclusion list.

Why?
====

Why would we bother building this service, when any Mozilla service could take
another service like Stripe or Braintree, slap in a bit of JS and be done? In
no particular order:

* We want people selling services or products to focus on their product, not on
  the purchase story.
* One team can own the security, legal and privacy issues around payments.
* Prevent fracturing of payments by having each different team use a different
  payment provider.
* Appropriate data can be re-used across services. For example: a credit card
  stored with one service can be re-used with another.
* User iteractions including purchasing, receipt emails, refunds should be
  branded with Mozilla and conform to Mozilla accessibility, localisation and
  security standards.
* Provide a management interface for users that allows them to alter payment
  details, see purchase history and so on across all services.

Why not the drop-in UI?
+++++++++++++++++++++++

Braintree provides a `drop-in UI <https://developers.braintreepayments.com/guides/drop-in>`_.
Why not use that?

* At the moment Braintree doesn't support localisations. "Language support for
  18 languages on iOS and Android; English-only for web". It may in the future.
* The form won't be consistent with the Firefox Accounts UX
* We still need to make an iframe, do the auth and wrap everything up to show
  the drop-in ui.
* We are able to re-use open sourced components of Braintrees flow anyway. For
  example: `card validator <https://github.com/braintree/card-validator>`_.
* We lose the ability to track events on that form, do A/B testing and other
  improvements.

Other minor issues include:

* It means properties using the service have to trust the Braintree domain for
  loading resource as it alters the DOM. Just trusting our domain allows us
  to control the payment flow to meet Mozilla security standards.
* Complete control over the payment flow has historically been missing in
  previous implementations, this has actually cost Mozilla time and effort [*]_

.. _components-label:

Front end components
====================

There would be at least three main front end components.

Library
+++++++

Added to the site selling the product. The library allows the selling site to
easily bring up the iframe for completing the payment flow and then processing
the completed flow.

* *UX*: :ref:`buy flow <purchase-label>`
* *Documentation*: ...
* *Repository*: https://github.com/mozilla/payments-client
* *Uptime requirements*: will be hosted on the site selling the product.

Buy flow
++++++++

Shown in an iframe to the end user. Triggered by the library.

* *UX*: :ref:`buy flow <purchase-label>`
* *Documentation*: ...
* *Repository*: https://github.com/mozilla/payments-ui
* *Uptime requirements*: very high, if this goes out, purchasing will fail. Based also upon
  the uptime of the payment provider and all the stacks inbetween.

Management screens
++++++++++++++++++

Shown as a standard set of pages. Accessible at a URL and linked from the
Firefox Accounts profile page.

* *UX*: ...
* *Documentation*: ...
* *Repository*:
* *Uptime requirements*: high, this doesn't stop purchasing work but prevents
  later management.

Example site
++++++++++++

An example site that shows how

* *Repository*: https://github.com/mozilla/payments-example
* *Uptime requirements*: none, its an example.


Back end components
===================

Environment
+++++++++++

Contains the environment for running the services.

* *Repository*: https://github.com/mozilla/payments-env

Service
+++++++

Does authentication and acts a broker between the buy flow and solitude.

* *Documentation*: `service docs <http://payments-service.readthedocs.org/en/latest/>`_
* *Repository*: https://github.com/mozilla/payments-service
* *Uptime requirements*: very high.

Solitude
++++++++

Stores a limited amount of payment information and interacts with the payment
provider.

* *Documentation*: `solitude docs <https://solitude.readthedocs.org>`_
* *Repository*: https://github.com/mozilla/solitude
* *Uptime requirements*: very high.

.. [*] Further information available internally to Mozilla.
