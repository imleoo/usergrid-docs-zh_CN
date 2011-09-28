=============
API Endpoints
=============

Please remember to include an access_token with all API calls unless the
documentation explicitly says it's not necessary.

-------------
Post Messages
-------------

.. http:method:: POST /{app_id}/queues/{path/to/queue}?cascade=true&ql=

   :arg app_id: application UUID or application name
   :arg path/to/queue: a queue path
   :optparam cascade: (false,1..n,all;default=1) what level of subscribers to distribute message to
   :optparam ql: a query language statement to specify subscribers to deliver to based on queue properties
   :Request Body: Either a single set of message properties:

   .. code-block:: js

     { "foo" : "bar", "a" : { "b" : 1, "c" : 2 }, timestamp : 1314034739489 }

   Or an array of message properties in order to post multiple
   messages:

   .. code-block:: js

     [ { "foo" : "bar" }, { "foo" : "baz"} ]

   :Response Body:

   .. code-block:: js

     {
       "status" : "ok",
       "path" : "<path/to/queue>",
       "queue" : "<queue Id>",
       "messages" : [
           {
               "foo" : "bar",
               "a" : {
                   "b" : 1,
                   "c" : 2
               },
               "timestamp" : <timestamp>,
               "uuid" : "<new_message_uuid>"
           }
       ],
       "last" : "<uuid_of_last_message>",
     }

   Post a message to the specified queue path. Queues are specified by
   hierachical paths delimited by slashes. *Queues do not need to be defined
   before being used, simply posting to a queue path creates the queue if it
   doesn't already exist*.
   
   Message contents are specified in JSON format. Any valid JSON object can be
   stored in message, regardless of level of complexity, not just name/value
   pairs.
   
   Messages by default cascade to subscribers. This is the equivalent of
   specifying cascade=1. If you want to prevent this, specify cascade=false.
   To have messages cascade to subscribers of subscribers, specify cascade=2.
   To have messages cascade down the complete subscriber tree, specify
   cascade=all.

   It is optional for the calling application to provide a timestamp for new
   messages as one of the values in the message properties sent in the request
   body. If a timestamp is not provided, the system will generate one. It is
   recommended that, if possible, to let the system assign timestamp values
   rather than creating them in your application as to ensure that there are
   no issues with differences between clock accuracy on the local device or
   computer and the server.

------------
Get Messages
------------

.. http:method:: GET /{app_id}/queues/{path/to/queue}?consumer=&last=&time=&prev=&next=&limit=&pos=&update=&synchronized=

   :arg app_id: application UUID or application name
   :arg path/to/queue: a queue path
   :optparam consumer: a consumer id (either a UUID returned by the service or an arbitrary string)
   :optparam last: the last message id retrieved from the queue, return messages after this one
   :optparam time: the timestamp to start returning messages from
   :optparam prev: the number of previously retrieved messages to return
   :optparam next: the number of new messages to return
   :optparam limit: the total number of messages to return
   :optparam pos: (start,end,last,consumer;default=consumer) where in the queue to start returning messages from
   :optparam update: (true,false;default=true) whether to update the position after retrieving the messages
   :optparam synchronized: (true,false;default=false) whether to ensure that requests are executed sequentially across clients
   :Response Body:

   .. code-block:: js

     {
       "status" : "ok",
       "path" : "<path/to/queue>",
       "queue" : "<queue_id>",
       "messages" : [
           {
               "foo" : "bar",
               "a" : {
                   "b" : 1,
                   "c" : 2
               },
               "timestamp" : <timestamp>,
               "uuid" : "<message_uuid>"
           }
       ],
       "last" : "<uuid_of_last_message>",
       "consumer" : "<consumer_id>"
     }

   Get messages from the specified queue path. The default operation is to
   return the next message after the last message that was retrieved.
   Additional messages can be specified by setting the next parameter which
   will return the specified number of new operations from the last message
   retrieved.

   The following example request that will return the next message in a queue
   every time it's invoked::

     GET /test-app/queues/my-queue

   Because no consumer is specified, every client app that makes that request
   will receive the next message with no two clients receiving the same
   message.

   This is an example of a request that will return the next message in a
   queue for a specific consumer. Different consumers will only see the
   messages they haven't seen before::

     GET /test-app/queues/my-queue?consumer=9e35aea7-cce5-11e0-9af9-9227e40e3559

   This example request will return a feed of the 10 most recent messages,
   suitable for a content feed or a social inbox::

     GET /test-app/queues/my-queue?pos=end&limit=10

   This example request will return up to 10 new messages and 3 old messages,
   perhaps what might be show when logging in to an instant messaging client
   similar to the way Skype does::

     GET /test-app/queues/my-queue?consumer=9e35aea7-cce5-11e0-9af9-9227e40e3559&prev=5&next=10

   Notice the consumer ID is provided, meaning that the position in the queue
   will be specific to the consumer.

--------------
Add Subscriber
--------------

.. http:method:: PUT /{app_id}/queues/{path/to/publisher_queue}/subscribers/{path/to/subscriber_queue}

   :arg app_id: application UUID or application name
   :arg path/to/publisher_queue: a queue path
   :arg path/to/subscriber_queue: a queue path
   :Response Body:

   .. code-block:: js

     {
       "status" : "ok",
       "path" : "<path/to/publisher_queue>",
       "queue" : "<publisher_queue_id>",
       "subscribers" : [
           {
               "path" : "path/to/subscriber_queue",
               "queue" : "subscriber_queue_id",
           }
       ],
       "last" : "<uuid_of_last_message>",
     }

   Add a single subscriber to the specified queue.

---------------------------
Add One Or More Subscribers
---------------------------

.. http:method:: PUT /{app_id}/queues/{path/to/publisher_queue}/subscribers

   :arg app_id: application UUID or application name
   :arg path/to/publisher_queue: a queue path
   :Request Body: Either a single subscriber:

   .. code-block:: js

     { "subscriber" : "<path/to/subscriber_queue>" }

   Or an array of message properties in order to post multiple
   messages:

   .. code-block:: js

     { "subscribers" : ["<path/to/subscriber_queue>", "<path/to/subscriber_queue>"] }

   :Response Body:

   .. code-block:: js

     {
       "status" : "ok",
       "path" : "<path/to/publisher_queue>",
       "queue" : "<publisher_queue_id>",
       "subscribers" : [
           {
               "path" : "path/to/subscriber_queue",
               "queue" : "subscriber_queue_id",
           }
       ],
       "last" : "<uuid_of_last_message>",
     }

   Add one or more subscribers to the specified queue.

-----------------
Delete Subscriber
-----------------

.. http:method:: DELETE /{app_id}/queues/{path/to/publisher_queue}/subscribers/{path/to/subscriber_queue}

   :arg app_id: application UUID or application name
   :arg path/to/publisher_queue: a queue path
   :arg path/to/subscriber_queue: a queue path
   :Response Body:

   .. code-block:: js

     {
       "status" : "ok",
       "path" : "<path/to/publisher_queue>",
       "queue" : "<publisher_queue_id>",
       "subscribers" : [
           {
               "path" : "path/to/subscriber_queue",
               "queue" : "subscriber_queue_id",
           }
       ],
       "last" : "<uuid_of_last_message>",
     }

   Remove a single subscriber to the specified queue.

------------------------------
Delete One Or More Subscribers
------------------------------

.. http:method:: DELETE /{app_id}/queues/{path/to/publisher_queue}/subscribers

   :arg app_id: application UUID or application name
   :arg path/to/publisher_queue: a queue path
   :Request Body: Either a single subscriber:

   .. code-block:: js

     { "subscriber" : "<path/to/subscriber_queue>" }

   Or an array of message properties in order to post multiple
   messages:

   .. code-block:: js

     { "subscribers" : ["<path/to/subscriber_queue>", "<path/to/subscriber_queue>"] }

   :Response Body:

   .. code-block:: js

     {
       "status" : "ok",
       "path" : "<path/to/publisher_queue>",
       "queue" : "<publisher_queue_id>",
       "subscribers" : [
           {
               "path" : "path/to/subscriber_queue",
               "queue" : "subscriber_queue_id",
           }
       ],
       "last" : "<uuid_of_last_message>",
     }

   Remove one or more subscribers from the specified queue.

---------------
Get Subscribers
---------------

.. http:method:: GET /{app_id}/queues/{path/to/publisher_queue}/subscribers?cascade=&start=&limit=&ql=

   :arg app_id: application UUID or application name
   :arg path/to/publisher_queue: a queue path
   :optparam cascade: (1..n,all;default=1) what level of subscribers to return
   :optparam start: the path of the first subscriber queue to return
   :optparam limit: the total number of messages to return
   :optparam ql: a query language statement to filter subscribers based on queue properties
   :Response Body:

   .. code-block:: js

     {
       "status" : "ok",
       "path" : "<path/to/publisher_queue>",
       "queue" : "<publisher_queue_id>",
       "subscribers" : [
           {
               "path" : "path/to/subscriber_queue",
               "queue" : "subscriber_queue_id",
           },
           {
               "path" : "path/to/subscriber_queue",
               "queue" : "subscriber_queue_id",
           },
           ...
       ],
       "last" : "<uuid_of_last_message>",
     }

   Get the subscribers of the specified queue. Specifying a cascade value
   allows you to retrieve downstream subscribers as well.

--------------------
Get Queue Properties
--------------------

.. http:method:: GET /{app_id}/queues/{path/to/queue}/properties

   :arg app_id: application UUID or application name
   :arg path/to/publisher_queue: a queue path
   :Response Body:

   .. code-block:: js

     {
       "path" : "<path/to/queue>",
       "queue" : "<queue_id>",
       "newest" : "<newest_message_id>",
       "oldest" : "<oldest_message_id>",
       "created" : <time_created>,
       "modified" : <time_modified>,
       "foo" : "bar",
       ...
     }

   Get the properties of the specified queue. This includes both the system
   and application defined properties.

-----------------------
Update Queue Properties
-----------------------

.. http:method:: PUT /{app_id}/queues/{path/to/queue}/properties

   :arg app_id: application UUID or application name
   :arg path/to/publisher_queue: a queue path
   :Request Body:

   .. code-block:: js

     {
       "foo" : "bar",
       "a" : {
           "b" : 1,
           "c" : 2
       },
       ...
     }

   Stores application defined queue properties. Queue properties are indexed
   and can be used to define which of the subscribing queues that a message
   posted to a publishing queue will be distributed to. For example, you might
   store a user's location as a property in their inbox queue and then you
   could deliver messages only to queues with a specific location.


