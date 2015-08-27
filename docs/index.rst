====================================
Payments for Firefox Accounts
====================================

Contents:

.. toctree::
   :maxdepth: 2

   design/overview.rst
   contributing.rst
   design/design.rst
   design/ux.rst
   design/emails.rst
   design/api/index.rst
   testing.rst
   localisation.rst
   servers.rst
   onboarding.rst

This is the general root documentation source for all Payments for Firefox
Accounts.

Filing bugs:

* Github on :ref:`each repository <components-label>` or `this repository <https://github.com/mozilla/payments/issues>`_ (preferred).
* `Bugzilla <https://bugzilla.mozilla.org/enter_bug.cgi?product=Cloud%20Services&component=Payments>`_ in the ``Cloud Services`` > ``Payments``.
* `For IT issues <https://bugzilla.mozilla.org/enter_bug.cgi?product=Cloud%20Services&component=Operations:%20Marketplace>`_ in the ``Cloud Services`` > ``Operations: Marketplace``.

Other:

* `Public wiki page <https://wiki.mozilla.org/CloudServices/Payments>`_
* `Public mailing list <https://mail.mozilla.org/listinfo/dev-payments>`_

Contributing to this document
=============================

If you are editing this documentation, you need to set yourself up to build the
docs as you make changes. Using Python and `pip <https://pip.pypa.io/en/stable/>`_,
install the dependencies from a shell
(you may wish to install them in a `virtualenv <https://pip.pypa.io/en/stable/>`_)::

    pip install -r requirements/docs.txt

Build the documentation::

    make -C docs/ html

Open ``docs/_build/html/index.html`` in your web browser.

You can also check for broken links like this::

    make -C docs/ linkcheck

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
