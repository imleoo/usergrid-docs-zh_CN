================
Basic Operations
================

---------------
Creating A User
---------------

.. http:method:: POST http://api.usergrid.com/my-app/users

*Request Body*

.. code-block:: js

    {
        "username" : "edanuff",
        "email" : "ed@anuff.com",
        "name" : "Ed Anuff",
        "password" : "test"
    }
    
*Response Body*

.. code-block:: js

    {
      "action" : "post",
      "application" : "4353136f-e978-11e0-8264-005056c00008",
      "params" : {
      },
      "path" : "/users",
      "uri" : "http://localhost:8080/4353136f-e978-11e0-8264-005056c00008/users",
      "entities" : [ {
        "uuid" : "7d1aa429-e978-11e0-8264-005056c00008",
        "type" : "user",
        "created" : 1317176452536016,
        "modified" : 1317176452536016,
        "activated" : true,
        "email" : "ed@anuff.com",
        "metadata" : {
          "path" : "/users/7d1aa429-e978-11e0-8264-005056c00008",
          "sets" : {
            "rolenames" : "/users/7d1aa429-e978-11e0-8264-005056c00008/rolenames",
            "permissions" : "/users/7d1aa429-e978-11e0-8264-005056c00008/permissions"
          },
          "collections" : {
            "activities" : "/users/7d1aa429-e978-11e0-8264-005056c00008/activities",
            "feed" : "/users/7d1aa429-e978-11e0-8264-005056c00008/feed",
            "groups" : "/users/7d1aa429-e978-11e0-8264-005056c00008/groups",
            "roles" : "/users/7d1aa429-e978-11e0-8264-005056c00008/roles",
            "following" : "/users/7d1aa429-e978-11e0-8264-005056c00008/following",
            "followers" : "/users/7d1aa429-e978-11e0-8264-005056c00008/followers"
          }
        },
        "name" : "Ed Anuff",
        "username" : "edanuff"
      } ],
      "timestamp" : 1317176452528,
      "duration" : 52
    }

