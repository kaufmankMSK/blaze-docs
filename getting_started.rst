============================================
Getting Started
============================================

API Reference
=============
The MSK Blaze API is organized around REST. Each FHIR resource type currently supports `read <http://build.fhir.org/http.html#read>`_ and basic `search <http://build.fhir.org/http.html#search>`_ capabilities.


Authentication
==============

In order to make use of Blaze, you'll need to be set up as an MSK "partner" so that you can consume clinical research data. If you would like to request access to data 
for a research study at MSK, please send a request to **rtmcritds@mskcc.org**. 


Authentication is based on the Client Credentials grant. This means that clients will need to generate an access token and supply it in the headers of each request being made.
Once you are established as a partner, you'll be given a ``client_id`` and a ``client_secret``, which you will use for generating tokens and using them to make
authenticated requests to the server.


Generating Tokens
=================

To generate access tokens, partners need to make a POST request to the appropriate endpoint using their ``client_id`` and ``client_secret``:

Base URL (Test)
--------

.. code-block:: ReST

    https://webapit.mskcc.org/

Base URL (Production)
--------

.. code-block:: ReST

    Coming Soon...

**Request**

.. code-block:: ReST

    POST /auth/oauth/v2/token
    Content-Type: application/x-www-form-urlencoded
    client_id=YYYY
    client_secret=XXXX
    grant_type=client_credentials
    scope=oob


**Response**

.. code-block:: json

    {
        "access_token": "7ef1949a-fab1-4600-89ca-fbeb499ef68f",
        "token_type": "Bearer",
        "expires_in": 3600,
        "scope": "oob"
    }

Making Requests
===============

To make requests, include the bearer token you generated in your requests as a part of the ``Authorization`` header. Consider the following request for
retrieving observations for a research study:

**Request**

.. code-block:: ReST

    GET /api360/v2/clinical/observations?researchstudy=TEST&category=laboratory&_count=5
    -H Authorization: Bearer {access_token}


**Response**
*(some attributes omitted for brevity)*

.. code-block:: json

    {
        "resourceType": "Bundle",
        "identifier": {
            "system": "https://datapedia.mskcc.org/index.php/IDB.PROTOCOL",
            "value": "TEST"
        },
        "type": "searchset",
        "total": 20,
    }

Authorization
=============

All data access is restricted on a per protocol basis. It is assumed that incoming requests to Blaze always contain a ``researchstudy`` parameter, 
which identifies what research study the client is requesting data for.

Your ``client_id`` determines what research studies you have access to at MSK. This information is used in combination 
with the ``researchstudy`` parameter to authorize requests. If a partner has sufficient authority to access protocol data, the request will proceed - 
otherwise they will get an error message.




