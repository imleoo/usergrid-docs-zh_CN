==============
Basic Concepts
==============

--------
Messages
--------

A Message is a simple JSON object. All it has to contain is a UUID that we
assign it and a timestamp that either we or your application can provide.
However, it can contain any additional JSON data that you want to store in it,
including values, arrays, other objects.  This makes it a simple proposition
to use Message for storing everything from Tweets coming in from a raw
Twitter stream to application state or communications between between servers.
*All Messages are permanently stored and are immutable.*

------
Queues
------

A Queue is a timestamp-ordered set of messages. Typical, most operations on a
queue will be performed by a producer application that posts messages to the 
queue and a consumer application which connects to the queue and retrieves
new messages as they become available.


First-In-First-Out (FIFO)
-------------------------

Queues maintain state regarding the most recent messages delivered to the
queue and the most recent read from the queue. A typical example of this in
use is to have a queue of work tasks, and then have each worker go to the
queue to get the next available work task, perform the task, and then return
to the queue to get another task to process. Messages would be handed out
sequentially to each worker and no two workers would receive the same task.

Use Random Access To Build Feeds 
--------------------------------

It's important to understand that, unlike other messages queue systems you
might have used, that our queues don't have to be "black boxes". While our
queues can be accessed in the classic sequential FIFO model of traditional
message queues, they can also be accessed in a true random-access method. If
you do this, then the automatic management of consumer's positions in the
queue will not apply to the operations you make, but this lets you directly
use the queue to power things like feeds of sequential content. For example,
it's very trivial to construct a simple latest headlines widget that always
shows the last 10 headlines from the queue. Such a widget might be written in
Javascript, have no local state whatsoever, and consist of nothing more that
an API request to a message queue endpoint on our service requesting the 10
latest message. Such a widget would be able to function at virtually any level
of scale. When the widget made the request to the service, it would specify
that it wanted to service to ignore the queue position and to retrieve a set
of messages from the end of the queue. The specifics of how this is done are
documented in later sections.

Queues can have any JSON data as metadata
-----------------------------------------

Finally, Queues are also full JSON objects and can have any arbitrary
properties associated with them. These properties can be used as criteria in
message delivery. For example, a message can be delivered to a subscriber
queue where "location contains 'new york'". This is particularly useful when
configuring queues to subscribe to other queues and being able to define rules
for message forwarding. These topics will get discussed in the section on
Subscriptions And Broadcasting later.

Searching Queues
----------------

If a Message is indexed, then all application-defined properties are indexed
and searchable. Queue search results are always time-ordered and are retrieved
according to the position and number of results that you specify. For example,
if you specify that you want messages where a property contains a certain
keyword and you retrieve the next three messages that contain that keyword,
then the position in the queue will be advanced past the next three messages
that meet that constraint. This means that the position will be advanced past
as many messages that don't mean that constraint until three messages are
found or the end of the queue is reached.

Queue Limits
------------

Queues are defined to hold a "nigh infinite" amount of messages. However,
there are certain limits that you should be aware of. If you're going to be
storing over a thousand messages per second on a sustained basis over a twenty
four hour period (for example, from a Twitter firehose feed), you should
contact us to make special arrangements. Also note that maintaining indexes
will further reduce this depending on how much text content you're storing.

-----------------
Consumers & State
-----------------

Consumers refer to application clients that operate on message queues in order
to retrieve or take delivery of messages. When you access the message queue
service as a consumer, you can do so anonymously or by providing a consumer
ID. An anonymous operation uses the state of the queue to determine the next
message you should see and what messages you've already been delivered. If you
tag a message, that tag is stored with the queue. For many applications, it
makes sense to assign a queue to every user, for example as a user's inbox. In
this case, there's no real need for managing the consumer's state separate
from the queue.

Queues With Multiple Consumers
------------------------------

However, if you want to enable multiple consumers to independently access one
ore more queues and have the consumers able to independently retrieve
messages, where every consumer can be at a different position in the queue,
and they can reach retrieve their own copy of the message, or have every
consumer able to tag messages independently, then you need to makes sure your
consumer identifies itself by passing the same consumer ID with every API
request.

Consumers and Metadata
----------------------

Although Messages are immutable, metadata can be attached to messages. A
simple example of this would be "favoriting" a message. If you're logged into
as an identified consumer, then every time you favorite a message in a
queue, the metadata to represent this is attached to the consumer, If you're
logged in anonymously then that metadata would be visible and modifiable to
anyone in the queue. Again, the desired behavior will be very
application-specific. If you're giving every user their own queue as an inbox,
then there's no need for any additional state management for consumers. If
there's a group queue that multiple consumers can interact with. then you'll
want the service to maintain separate state for every consumer.
