Overview
--------

Front end components
====================

There would be at least three main front end components.

Library
+++++++

Added to the site selling the product. The library allows the selling site to
easily bring up the iframe for completing the payment flow and then processing
the completed flow.

* *UX*: none
* *Documentation*: ...
* *Repository*: ...
* *Uptime*: will be hosted on the site selling the product.

Buy flow
++++++++

Shown in an iframe to the end user. Triggered by the library.

* *UX*: :ref:`buy flow <purchase-label>`
* *Documentation*: ...
* *Repository*: ...
* *Uptime*: very high, if this goes out, purchasing will fail. Based also upon
  the uptime of the payment provider and all the stacks inbetween.

Management screens
++++++++++++++++++

Shown as a standard set of pages. Accessible at a URL and linked from the
Firefox Accounts profile page.

* *UX*: ...
* *Documentation*: ...
* *Repository*:
* *Uptime* :high, this doesn't stop purchasing work but prevents later
  management.


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
