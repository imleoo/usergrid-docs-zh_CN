================================
Content Filters & Delivery Rules
================================

It's often desirable to be able to specify either which types of messages
should b posted to a queue and it's subscribers or to specify which
subscribers of a queue should receive a copy of a message.

Every queue can have a set of filters provided that will be evaluated against
new messages to determine whether the message should be added to the queue.
For example, you might be posting stock quotes from a realtime stream to a
queue and you'd want subscribers to the stock quote queue to only receive
stocks in their individual portfolios.

The reverse of this is also possible. When a message is posted, you can
specify a query that will be used to determine which subscribers of the queue
you're posting to should receive the message as well. For example, you could
specify that you only wanted users in a specific zipcode to receive a message.

