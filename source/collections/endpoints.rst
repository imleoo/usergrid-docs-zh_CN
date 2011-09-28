=============
API Endpoints
=============

Please remember to include an access_token with all API calls unless the
documentation explicitly says it's not necessary.

-------------------
Create A New Entity
-------------------

.. http:method:: POST /{app_id}/{collection}

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :Request Body: Either a single set of entity properties:

   .. code-block:: js

     { "foo" : "bar", "a" : { "b" : 1, "c" : 2 } }

   Or an array of entity properties in order to create multiple
   entities:

   .. code-block:: js

     [ { "foo" : "bar" }, { "foo" : "baz"} ]

   :Response Body:  Example of newly created entity result:

   .. code-block:: js

    {
        "action": "post",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }

   Post an entity to the specified collection. *Collections do not need to be defined
   before being used, simply posting to a collection creates the collection if it
   doesn't already exist*.
   
   Entity contents are specified in JSON format. Any valid JSON object can be
   stored in entity, regardless of level of complexity, not just name/value
   pairs.
   
   Entity types are defined by the collection being posted to.  For example, posting
   to a collection named "cats" will create entities of type "cat".

   *All user-defined properties are indexed*.  You don't need to do anything special
   to have your entities fully indexed by the system.  Strings that contain multiple
   words are keyword indexed as well.

   For all application-specific entities, you can provide a property called
   "name" that you can use to retrieve the entity rather than it's UUID. The
   value for the "name" property must be unique. Some system-provided
   entities, like User, define a different property to be used for finding an
   entity. For example, User entities specify the "username" property rather
   than "name". Refer to the Entity Reference for details.

------------------------------------
Get An Entity By Entity UUID Or Name
------------------------------------

.. http:method:: GET /{app_id}/{collection}/{uuid|name}

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string uuid|name: an entity uuid or name
   :Response Body: Example of entity result:

   .. code-block:: js

    {
        "action": "post",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }

   Gets the message with the specified UUID or entity lookup name property.

   For any application-specific entity type, if you have a "name" property
   specified for the entity, you can provide that instead of the uuid to
   lookup the entity. For system-provided entities, there might be a different
   property than "name" defined for doing lookups. For example, the User
   entity defines "username" as it's lookup property. Refer to the Entity
   Reference to see if there's an alternate lookup property defined for any
   system-provided entity types you're using.

----------------
Update An Entity
----------------

.. http:method:: PUT /{app_id}/{collection}/{uuid|name}

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string uuid|name: an entity uuid or name
   :Request Body: Either a single set of entity properties:

   .. code-block:: js

     { "alpha" : "bravo" }

   :Response Body: Example of updated entity result:

   .. code-block:: js

    {
        "action": "post",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "alpha": "bravo",
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }

   Update an entity in a collection.  The new property values are stored in the entity.

---------------------------------------
Delete An Entity By Entity UUID Or Name
---------------------------------------

.. http:method:: GET /{app_id}/{collection}/{uuid|name}

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string uuid|name: an entity uuid or name
   :Response Body:  Example of deleted entity result:

   .. code-block:: js

    {
        "action": "post",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }

   Deletes the message with the specified UUID or entity lookup name property.

   Returns the contents of the deleted entity.

------------------
Query A Collection
------------------

.. http:method:: GET /{app_id}/{collection}?ql=&reversed=&start=&cursor=&limit=&permission=&filter=

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :optparam string ql: a query in the query language
   :optparam string reversed: return results in reverse order
   :optparam uuid start: the first entity UUID to return
   :optparam string cursor: an encoded representation of the query position for paging
   :optparam integer limit: the number of results to return (default=10)
   :optparam string permission: a permission type
   :optparam string filter: a condition to filter on (multiple allowed)
   :Response Body:  Example of single entity query result:

   .. code-block:: js

    {
        "action": "get",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }

   Retrieves the set of entities that meet the query criteria or all entities
   (up to limit or 10) if no query or filters are provided. See the Queries
   section for details on the options for querying or filtering.

----------------------------
Update A Collection By Query
----------------------------

.. http:method:: PUT /{app_id}/{collection}?ql=&reversed=&permission=&filter=

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :optparam string ql: a query in the query language
   :optparam string permission: a permission type
   :optparam string filter: a condition to filter on (multiple allowed)
   :Request Body: A set of entity properties:

   .. code-block:: js

     { "alpha" : "bravo" }

   :Response Body:  Example result of single entity updated by query:

   .. code-block:: js

    {
        "action": "get",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "alpha": "bravo",
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }

   Updates all entities that meet the specified criteria.

--------------------------------------------
Query An Entity's Collections or Connections
--------------------------------------------

.. http:method:: GET /{app_id}/{collection}/{entity_id}/{relationship}?ql=&type=&reversed=&start=&cursor=&limit=&permission=&filter=

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (i.e. "likes")
   :optparam string ql: a query in the query language
   :optparam string type: the entity type to return
   :optparam string reversed: return results in reverse order
   :optparam string connection: the connection type (i.e. 'likes')
   :optparam uuid start: the first entity UUID to return
   :optparam string cursor: an encoded representation of the query position for paging
   :optparam integer limit: the number of results to return (default=10)
   :optparam string permission: a permission type
   :optparam string filter: a condition to filter on (multiple allowed)
   :Response Body:  Example of single entity result:

   .. code-block:: js

    {
        "action": "get",
        "application": "438a1ca1-cf9b-11e0-bcc1-12313f0204bb",
        "params": {},
        "path": "/things",
        "uri": "http://api.usergrid.com/438a1ca1-cf9b-11e0-bcc1-12313f0204bb/things",
        "entities": [
            {
                "uuid": "4c469e8a-d8ed-11e0-bcc1-12313f0204bb",
                "type": "thing",
                "created": 1315357451966016,
                "modified": 1315357451966016,
                "a": {
                    "b": 1,
                    "c": 2
                },
                "alpha": "bravo",
                "foo": "bar",
                "metadata": {
                    "path": "/things/4c469e8a-d8ed-11e0-bcc1-12313f0204bb"
                }
            }
        ],
        "timestamp": 1315357451949,
        "duration": 52
    }

   Retrieves the set of entities that meet the query criteria or all entities
   (up to limit or 10) if no query or filters are provided. See the Queries
   section for details on the options for querying or filtering.

------------------------------------------------------
Put An Entity Into A Collection Or Create A Connection
------------------------------------------------------

.. http:method:: POST /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_id}

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string first_entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (i.e. "likes")
   :arg uuid second_entity_id: entity UUID

   If {relationship} is a collection (such as a Group's "users" collection),
   adds the second entity to the first entity's collection of the specified
   name. In the case of a Group, this is how you'd add Users as members of the
   Group::

    POST /my-app/groups/employees/users/ed@anuff.com

   If {relationship} is not defined for the entity, a connection is create.
   For example, if the relationship is defined as "likes", the second entity
   is connected to the first with a "likes" relationship::

    POST /my-app/users/ed@anuff.com/likes/4c469e8a-d8ed-11e0-bcc1-12313f0204bb

----------

.. http:method:: POST /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_type}/{second_entity_id}

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string first_entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (i.e. "likes")
   :arg string second_entity_type: the type of the second entity
   :arg uuid|string second_entity_id: entity UUID or entity name

   When creating a connection, if you specify the entity type for the second
   entity, then you can create the connection using the entity's name rather
   than it's uuid. For example::

    POST /my-app/users/ed@anuff.com/likes/food/pizza

---------------------------------------------------------
Remove An Entity From A Collection Or Delete A Connection
---------------------------------------------------------

.. http:method:: DELETE /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_id}

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string first_entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (i.e. "likes")
   :arg uuid second_entity_id: entity UUID

   If {relationship} is a collection (such as a Group's "users" collection),
   removes the second entity from the first entity's collection of the
   specified name. In the case of a Group, this is how you'd remove Users as
   members of the Group::

    DELETE /my-app/groups/employees/users/ed@anuff.com

   For connections, provide the connection type. For example, if the
   relationship is defined as "likes", the second entity is connected to the
   first with a "likes" relationship::

    DELETE /my-app/users/ed@anuff.com/likes/4c469e8a-d8ed-11e0-bcc1-12313f0204bb

----------

.. http:method:: DELETE /{app_id}/{collection}/{first_entity_id}/{relationship}/{second_entity_type}/{second_entity_id}

   :arg uuid|string app_id: application UUID or application name
   :arg string collection: a collection name
   :arg uuid|string first_entity_id: entity UUID or entity name
   :arg string relationship: a collection name or connection type (i.e. "likes")
   :arg string second_entity_type: the type of the second entity
   :arg uuid|string second_entity_id: entity UUID or entity name

   When deleting a connection, if you specify the entity type for the second
   entity, then you can delete the connection using the entity's name rather
   than it's uuid. For example::

    DELETE /my-app/users/ed@anuff.com/likes/food/pizza





