Storage API
###########

For this examples let's say:

- Buyers log into the service with Firefox Account Bearer Tokens
- Seller apps log into the service with Hawk
- Payment App log into the service with Basic Auth


As a user, I want to retrieve my information about my purchases
===============================================================

.. http:get:: /buckets/(bucket_id)/collections/(collection_id)/records

    **Example request**:

    .. sourcecode:: http

        GET /buckets/mozilla/collections/payments/records HTTP/1.1
        Authorization: Bearer <User Firefox Account Bearer Token>

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json; charset=UTF-8

        {
          "data": [
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

    :statuscode 200: Ok no error.

Here the filtering on the user id is implicit because we are connected
as the buyer and she can only retrieve records she has got access
to.


As the payments app, I want to submit new payment information for a given user
==============================================================================

.. http:post:: /buckets/(bucket_id)/collections/(collection_id)/records

    **Example request**:

    .. sourcecode:: http

        POST /buckets/mozilla/collections/payments/records HTTP/1.1
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

    **Example response**:

    .. sourcecode:: http

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

    :statuscode 201: The record have been created.



As the payments app, I want to remove an existing payment from the system
=========================================================================

.. http:delete:: /buckets/(bucket_id)/collections/(collection_id)/records/(record_id)

    **Example request**:

    .. sourcecode:: http

        DELETE /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f HTTP/1.1
        Authorization: Basic <PaimentApp Basic Auth Credentials>

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 204 No Content

    :statuscode 204: The record have been deleted without error.


As the payments app, I want to edit an existing payment
=======================================================

Replace the existing record, using PUT:

.. http:put:: /buckets/(bucket_id)/collections/(collection_id)/records/(record_id)

    **Example request**:

    .. sourcecode:: http

        PUT /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f HTTP/1.1
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

    **Example response**:

    .. sourcecode:: http

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

    :statuscode 200: Ok, no error


Modify some fields of the existing record using PATCH:

.. http:patch:: /buckets/(bucket_id)/collections/(collection_id)/records/(record_id)

    **Example request**:

    .. sourcecode:: http

        PATCH /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f HTTP/1.1
        Authorization: Basic <PaymentApp Basic Auth credentials>

        {
          "data": {
            "valid_until": 1437208555580
          }
        }

    **Example response**:

    .. sourcecode:: http

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

    :statuscode 200: Ok, no error


As the payments application I want to be able to alter all purchases
====================================================================

You'll need to do a BATCH operation with all the sub-operations in there.

- First get the list of records you want to modify.

.. http:get:: /buckets/(bucket_id)/collections/(collection_id)/records

    **Example request**:

    .. sourcecode:: http

        GET /buckets/mozilla/collections/payments/records?seller=hawk:144c22a77d75937740ec3c957fbdb1d1 HTTP/1.1
        Authorization: Basic <PaymentApp Basic Auth credentials>

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK

        {
          "data": [
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

    :query seller: Filter on the seller app identifier
    :statuscode 200: Ok, no error


- Then run a BATCH request:

If you want to add the ``read`` permission for the seller app to all
records of the app, you could use:

.. http:post:: /batch

    **Example request**:

    .. sourcecode:: http

        POST /batch HTTP/1.1
        Authorization: Basic <PaymentApp Basic Auth credentials>

        {
          "defaults": {
            "data": {
              "permissions": {
                "read": [
                  "hawk:144c22a77d75937740ec3c957fbdb1d1"
                ]
              }
            },
            "method": "PATCH"
          },
          "requests": [
            {
              "path": "/buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f"
            },
            {
              "path": "/buckets/mozilla/collections/payments/records/db4d95e1-c076-4848-950c-cf462b2631f0"
            }
          ]
        }


As the selling application I want to be able to access the purchase information for agivenuser for my application
=================================================================================================================

.. http:get:: /buckets/(bucket_id)/collections/(collection_id)/records

    **Example request**:

    .. sourcecode:: http

        GET /buckets/mozilla/collections/payments/records?buyer=fxa:5331be33303ccff19d3c49b6da276913 HTTP/1.1
        Authorization: Hawk mac="kDPC...=", hash="B0we...=", id="144c22a77d75937740ec3c957fbdb1d1", ts="1432030137", nonce="mQao38"

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK

        {
          "data": [
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

    :query buyer: Filter on the buyer user identifier
    :statuscode 200: Ok, no error


As the selling application, I cannot access other selling applications payments
===============================================================================

.. http:get:: /buckets/(bucket_id)/collections/(collection_id)/records

    **Example request**:

    .. sourcecode:: http

        GET /buckets/mozilla/collections/payments/records?seller=hawk:5331be33303ccff19d3c49b6da276913 HTTP/1.1
        Authorization: Hawk mac="kDPC...=", hash="B0we...=", id="144c22a77d75937740ec3c957fbdb1d1", ts="1432030137", nonce="mQao38"

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 200 OK

        {
          "data": []
        }

    :query seller: Filter on the seller app identifier
    :statuscode 200: no error, but also no items in that case


As a user, I should not be able to edit / add payments
======================================================

.. http:put:: /buckets/(bucket_id)/collections/(collection_id)/records/(record_id)

    **Example request**:

    .. sourcecode:: http

        PUT /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f HTTP/1.1
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

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 403 Forbidden

    :statuscode 403: Forbidden, the authenticated user cannot modifiy this record.


As the selling application I should not be able to edit / add purchases
=======================================================================

.. http:put:: /buckets/(bucket_id)/collections/(collection_id)/records/(record_id)

    **Example request**:

    .. sourcecode:: http

        PUT /buckets/mozilla/collections/payments/records/dc86afa9-a839-4ce1-ae02-3d538b75496f HTTP/1.1
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

    **Example response**:

    .. sourcecode:: http

        HTTP/1.1 403 Forbidden

    :statuscode 403: Forbidden, the authenticated app cannot modifiy this record.


Any unauthorized operation will return a 403 HTTP response.
