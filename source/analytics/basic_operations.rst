================
Basic Operations
================

-------------------------------
Posting an Event With a Counter
-------------------------------

.. http:method:: POST http://api.usergrid.com/my-app/events

*Request Body*

.. code-block:: js

    {
        "counters" : {
            "button_clicks" : 1
        },
        "timestamp" : 0
    }
    
*Response Body*

.. code-block:: js

    {
         "action": "post",
         "path": "/events",
         "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/events",
         "entities": [
             {
                 "uuid": "39d41c46-d8e4-11e0-bcc1-12313f0204bb",
                 "type": "event",
                 "timestamp": 1315353555546016,
                 "counters": {
                     "button_clicks": 1
                 },
                 "created": 1315353555546016,
                 "modified": 1315353555546016,
                 "metadata": {
                     "path": "/events/39d41c46-d8e4-11e0-bcc1-12313f0204bb"
                 }
             }
        ],
        "timestamp": 1315353555537,
        "duration": 110
    }

-----------------------------------------------
Retrieving a Time Range of Values for a Counter
-----------------------------------------------

.. http:method:: GET http://api.usergrid.com/my-app/counters?start_time=1315119600000&end_time=1315724400000&resolution=day&counter=button_clicks

*Response Body*

.. code-block:: js

    {
        action: "get",
        uri: "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/counters",
        timestamp: 1315354369272,
        duration: 28,
        counters: [
            {
                name: "button_clicks",
                values: [
                    {
                        value: 2
                        timestamp: 1315180800000
                    },
                    {
                        value: 1
                        timestamp: 1315267200000
                    },
                    {
                        value: 1
                        timestamp: 1315353600000
                    }
                ]
            }
        ]
    }

