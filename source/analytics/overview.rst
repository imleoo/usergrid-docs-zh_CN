
==============
Basic Concepts
==============

--------
Counters
--------

Usergrid keeps track of key metrics for your applications in the form of
"counters". Counters represent a numeric value that gets incremented in
various ways. The total number of users in your app, the number of entities in
a collection, and the amount storage data used by your apps are all forms of
system-defined counters that Usergrid maintains for each application it
manages. In addition, you can define you own counters on the fly at any time.
For example, you want to know how many times people click on the help button
in your app, you simply start incrementing a counter named "help_button" via
the API.

How Counters Are Used
---------------------

The following are examples of things will perform counter operations:

  * Adding an entity will increment the total number of entities in the system
    as well as the total number of entities of that type
  * Every API operation will increment counters for the total number of bytes
    sent or received
  * Posting an event will increment counters for the event, for the user and
    group referenced in the event, for the category and keys in the event, and
    for any name/value pairs in the event
  * Posting a message to a queue will increment counters for the queue for any
    subscribers of that queue, for any categories and keys, and for name/value
    pairs in the message

-----------
Time Series
-----------

Simply knowing the total value of a counter is not very valuable, so behind
the scenes, Usergrid increments a set of counters so that at any time, you can
view this data over any time interval or level of granularity. For example,
let's say you're incrementing a counter every time someone launches your
application. You might be interested in which days of the week the application
sees the most usage. This is as simple as looking at the time series over a
set of weeks and looking at it split into daily intervals. You'll quickly see
which are your peak days of usage. For some applications, it makes sense to
view usage across the day, so you can see if it's being used more in the
mornings or the evenings. For business reporting, you may be more interested
in monthly or quarterly reporting. The important thing is that Usergrid is
able to provide you with the report data at a moments notice. All of this data
is maintained in realtime so it can be viewed instantly.

--------------
Categorization
--------------

As mentioned above, several parts of the Usergrid service feed the analytics
system. We'll describe how these work in a more detail later. However, all the
mechanisms that feed the analytics service also use various forms of
categorization when they keep track of metrics data.

What this means, for example, is that if we're keeping track of events logged
by an application, that we allow the application to provide a user id as well
as a group id and a set of other ways of categorizing and qualifying the data.
This means that, if you're keeping track of how often people use a feature,
you can not just find out the total number of users that used the feature, but
which groups contained users that made the most use of the feature, or you
could provide a location with the event, so you could see how often a feature
was used by people in San Francisco versus Los Angeles.

These different mechanisms of categorization can be combined with time-series
when performing report queries, and, because of the way that Usergrid
maintains this data, there's no overhead when performing reports.
