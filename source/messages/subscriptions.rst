============================================
Delivering Messages To One Or More Consumers
============================================

Many use cases require the ability to deliver a message to a number of
recipients. Other message queue systems make a distinction between normal
queues for one-to-one operations and then use a separate mechanism called a
topic for one to many broadcasting. We allow regular queues to be used for
one-to-many distribution through two different mechanisms:

  * Multiple consumers can read messages from the same queue and each
    consumers can maintain their own position in the queue as well as other
    consumer-specific metadata.
  * Queues can subscribe to other queues so that each consumer can be given
    it's own individual queue to read from.

----------------------------------------------
Multiple Consumers Reading from a Single Queue
----------------------------------------------

When reading from a queue, you can specify the position to read from as either
"last", meaning to read from the last point that anyone read from the queue,
or "consumer", meaning the last point that the consumer read from the queue.
In a situation where you have multiple consumer "workers" that are consuming
events from a queue, you would use the "position=last" option when retrieving
messages. If you were broadcasting messages where every consumer should
receive every message, you would save your consumer ID and use it every time
you connected, specifying the "position=consumer" (this is the default when a
consumer ID is provided).

----------------------------------
Queues Subscribing To Other Queues
----------------------------------

A queue can have any number of other queues subscribe to it. When a message is
posted to the first queue, a copy of it is placed into every subscribing
queue. This makes it possible to create queues that aggregate messages from a
number of source queues. This allows you to create things like inboxes or
social functionality such as "following" where a user would have a feed
containing all the activities of the users they follow. The user's feed would
be a queue that subscribed to all of the queues of the users they followed.

Gateway Queues
--------------

Subscriptions are also useful for distributing messages to special gateway
queues for relaying to other message-oriented services such as Twitter or
Facebook, or the Apple Push Notification Service and Android Cloud to Device
Messaging. For example, to relay messages to an iOS device, you would add the
following queue as a subscriber to your user's inbox queue:

  /apns/*<ios_device_id>*

For Android applications, the subscriber would be:

  /c2dm/*<android_registration_id>*

For relaying to Twitter, the subscriber would be:

  /twitter/*<twitter_user_id>*

Gateway queues are described in more detail in the appendix and often require
additional configuration to work correctly.

