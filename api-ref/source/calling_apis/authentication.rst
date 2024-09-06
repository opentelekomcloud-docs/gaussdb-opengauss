:original_name: opengauss_api_0010.html

.. _opengauss_api_0010:

Authentication
==============

Token authentication must be performed to call APIs.

Token-based authentication: Requests are authenticated using a token.

Token Authentication
--------------------

.. note::

   The validity period of a token is 24 hours. If a token is required, the system caches the token to avoid frequent calling.

A token specifies temporary permissions in a computer system. During API authentication using a token, the token is added to requests to get permissions for calling the API.

.. code-block::

   {
        "auth": {
            "identity": {
                "methods": [
                    "password"
                ],
                "password": {
                    "user": {
                        "name": "username",
                        "password": "********",
                        "domain": {
                            "name": "domainname"
                        }
                    }
                }
            },
            "scope": {
                "project": {
                    "name": "xxxxxxxx"
                }
            }
        }
    }

In :ref:`Making an API Request <opengauss_api_0009>`, the process of calling the API used to `obtain a user token <https://docs.otc.t-systems.com/en-us/api/iam/en-us_topic_0057845583.html>`__ is described.

After a token is obtained, add the **X-Auth-Token** header field must be added to requests to specify the token when calling other APIs. For example, if the token is **ABCDEFJ....**, **X-Auth-Token: ABCDEFJ....** can be added to a request as follows:

.. code-block:: text

   POST https://{{Endpoint}}/v3/auth/projects
   Content-Type: application/json
   X-Auth-Token: ABCDEFJ....
