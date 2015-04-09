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
