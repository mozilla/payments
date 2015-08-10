.. _configuration-label:

Configuration
=============

Product configuration occurs in the `payments-config repository <https://github.com/mozilla/payments-config/>`_.

Configuration is stored in a `python file <https://github.com/mozilla/payments-config/blob/master/payments_config/products.py>`_, JSON, translations and other formats are generated from that.

Example:

.. code-block:: python

    'mozilla-concrete': {
        'email': 'support@concrete.mozilla.org',
        'name': _('Mozilla Concrete'),
        'url': 'http://pay.dev.mozaws.net/',
        'terms': 'http://pay.dev.mozaws.net/terms/',
        'products': [
            {
                'id': 'brick',
                'description': _('Brick'),
                'amount': '10.00',
            },
            {
                'id': 'mortar',
                'description': _('Mortar'),
                'amount': '5.00',
                'img': ('https://raw.githubusercontent.com/mozilla'
                        '/payments-config/master/payments_config'
                        '/assets/mortar.png'),
            }
        ]
    }

* ``email``: a support address for the selling site
* ``name``: a general name for the selling site, shown to buyers
* ``url``: a URL to the selling site
* ``terms``: a URL to the selling site terms
* ``products``: an array of products for sale
* ``products.id``: a unique id for the product, not shown to buyers
* ``products.description``: a description shown to buyers of the product
* ``products.img``: optional image URL to an image to show to buyers
