
=============
Message Queue
=============

The message queue service is designed to provide the ability to distribute
messages to applications in a way that's both very easy to use as well as
highly scalable and with enough options to enable a variety of use cases. The
service is intended to have the flexibility to power server to client
notifications, social inboxes, content feeds, commenting services, as well as
traditional distributed message queue operations.

The message queue service is usable both directly and is also used
under-the-hood by the system of activity streams, event logging, and data
analytics. This section will primarily discuss using it directly, but even if
you don't plan to use it that way, if you plan to use things like activity
streams in your app, knowing how the system manages message queues will help.

.. toctree::
   overview
   basic_operations
   subscriptions
   filters
   gateways
   endpoints
   realtime



