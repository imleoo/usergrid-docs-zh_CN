================
Basic Operations
================

------------------
Creating a message
------------------

.. http:method:: POST http://api.usergrid.com/my-app/queues/foo/bar

*Request Body*

.. code-block:: js

    {
        "name" : "test"
    }
    
*Response Body*

.. code-block:: js

     {
         "status" : "ok",
         "path" : "/foo/bar/",
         "queue" : "1c184f38-9134-3400-b802-81315d9e7388",
         "messages" : [
             {
                 "name" : "test",
                 "timestamp" : 1314034739489,
                 "uuid" : "9e33d9e5-cce5-11e0-9af9-9227e40e3559"
             }
         ],
         "last" : "9e33d9e5-cce5-11e0-9af9-9227e40e3559",
     }

--------------------
Retrieving a message
--------------------

.. http:method:: GET http://api.usergrid.com/my-app/queues/foo/bar

*Response Body*

.. code-block:: js

    {
        "status" : "ok",
        "path" : "/foo/bar/",
        "queue" : "1c184f38-9134-3400-b802-81315d9e7388",
        "messages" : [
            {
                "name" : "test",
                "timestamp" : 1314034739489,
                "uuid" : "9e33d9e5-cce5-11e0-9af9-9227e40e3559"
            }
        ],
        "last" : "9e33d9e5-cce5-11e0-9af9-9227e40e3559",
    }

-----------------------------------
Getting the most recent 10 messages
-----------------------------------

.. http:method:: GET http://api.usergrid.com/my-app/queues/foo/bar?position=end&limit=10

*Response Body*

.. code-block:: js

    {
        "status" : "ok",
        "path" : "/foo/bar/",
        "queue" : "1c184f38-9134-3400-b802-81315d9e7388",
        "messages" : [
            {
                "name" : "test",
                "timestamp" : 1314034739489,
                "uuid" : "9e33d9e5-cce5-11e0-9af9-9227e40e3559"
            },
            {
                "name" : "test",
                "timestamp" : 1314034739489,
                "uuid" : "9e33d9e5-cce5-11e0-9af9-9227e40e3559"
            },
            {
                "name" : "test",
                "timestamp" : 1314034739489,
                "uuid" : "9e33d9e5-cce5-11e0-9af9-9227e40e3559"
            }
        ],
        "last" : "9e33d9e5-cce5-11e0-9af9-9227e40e3559",
    }

