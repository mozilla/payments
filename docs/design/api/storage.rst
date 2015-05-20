Storage API
###########

For this examples let's say:

- Buyers logs with Firefox Account Bearer Tokens
- Seller apps logs with Hawk
- Payment App logs with Basic Auth


As a user, I want to retrieve my information about my purchases
===============================================================

.. code-block:: http

    GET /buckets/mozilla/collections/payments/records
    Authorization: Bearer <User Firefox Account Bearer Token>


    HTTP/1.1 200 OK
    Content-Type: application/json; charset=UTF-8

    {
      "items": [
        {
            "id": "dc86afa9-a839-4ce1-ae02-3d538b75496f",
            "last_modified": 1432024555580,
            "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
            "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
            "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
            "added_on": 1432024555580,
            "valid_until": 1463560555580
        },
        {
            "id": "23160c47-27a5-41f6-9164-21d46141804d",
            "last_modified": 1430140411480,
            "title": "MoFo",
            "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
            "seller": "hawk:5851128c0c7d72c6845339599b46d092",
            "product": "bda9d953-dd81-427d-b7fc-7bcb0df9d666",
            "added_on": 1430140411480,
            "valid_until": 1432732411480
        }
      ]
    }


As the payments app, I want to submit new payment information for a given user
==============================================================================

.. code-block:: http

    POST /buckets/mozilla/collections/payments/records
    Authorization: Basic <PaimentApp Basic Auth Credentials>

    {
      "data": {
        "last_modified": 1432024555580,
        "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
        "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
        "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
        "added_on": 1432024555580,
        "valid_until": 1463560555580
      },
      "permissions": {
        "read": ["fxa:5331be33303ccff19d3c49b6da276913",
                 "hawk:144c22a77d75937740ec3c957fbdb1d1"]
      }
    }

    HTTP/1.1 201 Created
    Content-Type: application/json; charset=UTF-8

    {
      "data": {
        "id": "dc86afa9-a839-4ce1-ae02-3d538b75496f",
        "last_modified": 1432024555580,
        "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
        "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
        "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
        "added_on": 1432024555580,
        "valid_until": 1463560555580
      },
      "permissions": {
        "read": ["fxa:5331be33303ccff19d3c49b6da276913",
                 "hawk:144c22a77d75937740ec3c957fbdb1d1"]
      }
    }



As the payments app, I want to remove an existing payment from the system 
=========================================================================

.. code-block:: http

     DELETE   /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f
     Authorization: Basic <PaimentApp Basic Auth Credentials>
     
     HTTP/1.1 204 No Content


As the payments app, I want to edit an existing payment
=======================================================

Using PUT:

.. code-block:: http

     PUT   /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f
     Authorization: Basic <PaymentApp Basic Auth credentials>
     
    {
      "data": {
        "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
        "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
        "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
        "added_on": 1432024555580,
        "valid_until": 1437208555580
      },
      "permissions": {
        "read": ["fxa:5331be33303ccff19d3c49b6da276913",
                 "hawk:144c22a77d75937740ec3c957fbdb1d1"]
      }
    }

    HTTP/1.1 200 Ok
    {
      "data": {
        "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
        "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
        "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
        "added_on": 1432024555580,
        "valid_until": 1437208555580
      },
      "permissions": {
        "read": ["fxa:5331be33303ccff19d3c49b6da276913",
                 "hawk:144c22a77d75937740ec3c957fbdb1d1"]
      }
    }

Using PATCH:

.. code-block:: http

     PATCH   /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f
     Authorization: Basic <PaymentApp Basic Auth credentials>
     
    {
      "data": {
        "valid_until": 1437208555580
      }
    }

    HTTP/1.1 200 Ok
    {
      "data": {
        "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
        "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
        "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
        "added_on": 1432024555580,
        "valid_until": 1437208555580
      },
      "permissions": {
        "read": ["fxa:5331be33303ccff19d3c49b6da276913",
                 "hawk:144c22a77d75937740ec3c957fbdb1d1"]
      }
    }


As the payments application I want to be able to alter all purchases
====================================================================

You'll need to do a BATCH operation with all the sub-operations in there.

- First get the list of records you want to modify.

.. code-block:: http

    GET /buckets/mozilla/collections/payments/records?seller=hawk:144c22a77d75937740ec3c957fbdb1d1
    Authorization: Basic <PaymentApp Basic Auth credentials>
    
    HTTP/1.1 200 OK
    {
      "items": [
        {
            "id": "dc86afa9-a839-4ce1-ae02-3d538b75496f",
            "last_modified": 1432024555580,
            "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
            "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
            "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
            "added_on": 1432024555580,
            "valid_until": 1463560555580
        },
        {
            "id": "db4d95e1-c076-4848-950c-cf462b2631f0",
            "last_modified": 1430140411480,
            "title": "MoFo",
            "buyer": "fxa:465afecea6b565c85fd980a603747fec",
            "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
            "product": "ecd68f3c-984b-471c-a670-8411e5247358",
            "added_on": 1430140411480,
            "valid_until": 1432732411480
        }
      ]
    }


- Then run a BATCH requests.

If you want to add the ``read`` permission for the seller app to all records of the app, you could use:

.. code-block:: http

    POST /batch
    Authorization: Basic <PaymentApp Basic Auth credentials>
    
    {
      "defaults": {
      "method" : "PATCH",
      "data": {
        "permissions": {
          "read": ["+hawk:144c22a77d75937740ec3c957fbdb1d1"]
        }
      }
    },
    "requests": [
      {
        "path" : "/buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f"
      },
      {
        "path" : "/buckets/mozilla/collections/payments/records/db4d95e1-c076-4848-950c-cf462b2631f0"
      }
    ]
  }

As the selling application I want to be able to access the purchase information for agivenuser for my application
=================================================================================================================

.. code-block:: http

    GET /buckets/mozilla/collections/payments/records?buyer=fxa:5331be33303ccff19d3c49b6da276913
    Authorization: Hawk mac="kDPC...=", hash="B0we...=", id="144c22a77d75937740ec3c957fbdb1d1", ts="1432030137", nonce="mQao38"
    
    HTTP/1.1 200 OK
    {
      "items": [
        {
            "id": "dc86afa9-a839-4ce1-ae02-3d538b75496f",
            "last_modified": 1432024555580,
            "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
            "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
            "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
            "added_on": 1432024555580,
            "valid_until": 1463560555580
        }
      ]
    }


As the selling application, I cannot access other selling applications payments
===============================================================================

.. code-block:: http

    GET /buckets/mozilla/collections/payments/records?seller=hawk:5331be33303ccff19d3c49b6da276913
    Authorization: Hawk mac="kDPC...=", hash="B0we...=", id="144c22a77d75937740ec3c957fbdb1d1", ts="1432030137", nonce="mQao38"
    
    HTTP/1.1 200 OK
    {
      "items": []
    }


As a user, I should not be able to edit / add payments
======================================================

.. code-block:: http

     PUT   /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f
     Authorization: Bearer <User Firefox Account Bearer Token>
     
    {
      "data": {
        "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
        "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
        "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
        "added_on": 1432024555580,
        "valid_until": 1437208555580
      },
      "permissions": {
        "read": ["fxa:5331be33303ccff19d3c49b6da276913",
                 "hawk:144c22a77d75937740ec3c957fbdb1d1"]
      }
    }

    HTTP/1.1 403 Forbidden


As the selling application I should not be able to edit / add purchases
=======================================================================

.. code-block:: http

     PUT   /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f
     Authorization: Hawk mac="kDPC...=", hash="B0we...=", id="144c22a77d75937740ec3c957fbdb1d1", ts="1432030137", nonce="mQao38"
     
    {
      "data": {
        "buyer": "fxa:5331be33303ccff19d3c49b6da276913",
        "seller": "hawk:144c22a77d75937740ec3c957fbdb1d1",
        "product": "bb5bf35f-cb2b-40e7-b1ef-8b097bd550f2",
        "added_on": 1432024555580,
        "valid_until": 1437208555580
      },
      "permissions": {
        "read": ["fxa:5331be33303ccff19d3c49b6da276913",
                 "hawk:144c22a77d75937740ec3c957fbdb1d1"]
      }
    }

    HTTP/1.1 403 Forbidden

Basically an operation on something not authorized will result in a 403.
