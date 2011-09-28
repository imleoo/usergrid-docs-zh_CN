
======================
Websocket Realtime API
======================

The Websocket API allowa for asynchronous communications with the Usergrid API.

OAuth credentials are put in the querystring of the initial GET request since
the Javascript Websocket API provides no mechanism for specifying header
contents.

Once a connection is established, there are two mechanisms of usage.

----------------------
Async Request/Response
----------------------

The first is to simply access the API the same way as the REST interface
provides, except over the open websocket connection. The benefit is there is
much less overhead than with the REST API since there is no processing of
individual HTTP requests and authentication credentials are checked at the
time the websocket is opened as opposed to per-request as is necessary with
the stateless REST API.

-------------
Subscriptions
-------------

The second is to issue subscribe requests against entities and collections and
be notified asynchronously as those are updated. Subscriptions against the
activity inbox and message queue collections for either users, groups, or the
entire application allow connected applications to get realtime updates.

Connecting to an inbox or queue in the websocket url will automatically start
a subscription to that collection. For example, the following websocket url
will listen to a specific user's inbox::

  ws://api.usergrid.com:8088/chatapp/users/edanuff/inbox

This is the real-time equivalent of making a regular REST GET request to::

  http://api.usergrid.com/chatapp/users/edanuff/inbox

Please note that Usergrid listens to websockets on port 8088 rather than the
standard ports 80 or 443.

