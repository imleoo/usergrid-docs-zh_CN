====================
Basic API Operations
====================

Usergrid uses a pure REST API. See `Representational State Transfer
<http://en.wikipedia.org/wiki/Representational_State_Transfer>`_ at Wikipedia
for more information about the principles behind this type of API.

REST API's are built by thinking about resources, and the location of those
resources is described through paths that are intrinsically about collections
and elements in those collections. The challenge to build a REST API is to
figure out how to represent the data and the action in the form of a path to a
resource that can be created, retrieved, updated, or deleted.

Usergrid's data model is designed to be very easily accessed via a REST API.
For example, to access the user with the username "edanuff" in the application
"myapp", one would construct the following API request::

  http://api.usergrid.com/myapp/users/edanuff

To get a listing of everything he liked, use the following URL::

  http://api.usergrid.com/myapp/users/edanuff/likes

To limit it to entities of type "restaurant", use the following URL::

  http://api.usergrid.com/myapp/users/edanuff/likes/restaurant

-------------------------
Constructing API Requests
-------------------------

All API requests to the Usergrid live site are made to
http://api.usergrid.com. Please note that this will eventually be replaced
with https://api.usergrid.com.

Usergrid takes a fairly standard approach and interprets a URL resource path
as a list of names, UUIDs, or queries.

The basic path that follows the hostname is in the format of::

  /<application-uuid|application-name>/<collection-name>[/<entity-id|entity-name>]

From here on in, we'll describe the API starting from after the application
uuid or application name. For example, instead of this::

  http://api.usergrid.com/myapp/users

We'll just describe it as::

  /users

Remember that your actual HTTP REST requests need to have the full URL path.

Accessing Collections
---------------------

To request all the entities in a collection, just use the path of the
collection::

  /users

When you do this, you'll get the first 10 entities in a collection, sorted by
their entity uuid. Usually you'll either want to get more results, page
through results, or get a specific item in the collection. We'll describe how
thats done in the next section.

To create an item in the collection, you'd do a POST to the collection with
the data you wanted to new entity to contain in JSON format. This is described
more fully later in a later section.

Queryies and Parameters
-----------------------

There will usually be more entities in the collection than you can return with
one request, in which case, you'll want to issue a query::

  /users?filter=facebook.first_name%3D'john'

The above querystring value for the filter parameter is the url-encoded
version of::

  facebook.first_name='john'

You could construct multiple filters and the results would be AND'd together.
For example::

  /users?filter=facebook.first_name%3D'john'&filter=facebook.last_name%3D'doe'

Alternately, we also support an SQL-like query language, so you could do::

  select * where facebook.first_name='john' and facebook.last_name='doe'

The url-encoded version of that would be::

  /users?ql=select * where facebook.first_name%3D'john' and facebook.last_name%3D'doe'

Alternative to URL-encoding
---------------------------

As a convenience, we provide an alternative to conventional url-encoding of
parameters in queries.

For example, rather than this::

  /users?filter=facebook.first_name%3D'john'&filter=facebook.last_name%3D'doe'

we let you Json-encode queries into the URL::

  /users/{"filter" : ["facebook.first_name = 'john'", "facebook.last_name = 'doe'"]}

This is a non-standard convention that is optional and primarily provided to
make it easier for hand constructing test queries and we also use in in the
documentation to make some examples more clear to understand since the
url-encoded query values can be hard to follow. The Usergrid API can
understand queries in this format but we recommend using conventionally
formatted url-encoded queries within client libraries. The Javascript client
allows you to specify path queries in this fashion but transforms them into
standard querystring url-encoded parameters before issuing HTTP requests.

Accessing specific items in a Collection
----------------------------------------

To access a specific entity in a collection, you should use it's uuid or name.
For example, to look up the user with the username of 'john.doe', one could do
either of the following::
 
  /users/john.doe
  /users/1c18ca40-90b2-11e0-b91b-12313f0204bb

assuming that the user's uuid was 1c18ca40-90b2-11e0-b91b-12313f0204bb.

For example, the list of achievements for the user "ed"::

  /users/ed/achievements

The list of friends for the user "ed"::

  /users/ed/friends/

The list of achievements of the friends of the user "ed"::

  /users/ed/friends/achievements

Query Parameters
----------------

The following query parameters can be passed in the querystring (or in a
JSON-encode query):

==============  =======  ==========================================================
Parameter       Type     Description
==============  =======  ==========================================================
ql              string   a query in the query language
type            string   the entity type to return
reversed        string   return results in reverse order
connection      string   the connection type (i.e. 'likes')
start           string   the first entity UUID to return
cursor          string   an encoded representation of the query position for paging
limit           integer  the number of results to return
permission      string   a permission type
filter          string   a condition to filter on
==============  =======  ==========================================================

Queries are the place where most API's struggle a bit, because the logical
place to put a query is in the querystring, but what happens when you want to
filter or query a collection somewhere other than at the end of the path? The
URI specification addresses this by allowing a form of embedded querystrings
inside the paths called matrix parameters.

So, to Usergrid, this URL path::

  /users/ed/friends;filter="location eq new york"/achievements?filter="level eq mayor"

is interpreted as this::

  /users/ed/friends/location="new york"/achievements/level="mayor"

Again, we let you optionally use JSON-encoded queries and the URL path is human-readable::

  /users/ed/friends/{"filter" : "location='new york'"}/achievements/{"filter" : "level='new mayor'"}

-------------
Data Formats
-------------

Request Data
-------------

When creating or modifying entities, you will supply information in the form
of a JSON object in the body of the HTTP request.

As per REST conventions, you create an entity by POSTing to a collection and
you modify an entity by PUTting to an item in the collection.

When posting an entity, you provide the initial data you want for the entity::

  {
    "username" : "edanuff",
    "email" : "ed@anuff.com"
  }

You can create multiple entities in one request by supplying an array::

  [
    {
      "username" : "edanuff",
      "email" : "ed@anuff.com"
    },
    {
      "username" : "johndoe",
      "email" : "john.doe@gmail.com"
    }
  ]

Response Data
-------------

All API methods return a response object that typically contains an array of
entities::

  {
    "entities" : [
      ...
    ]
  }

Not everything can be included inside the entity, and some of the data that
gets associated with specific entities isn't part of their persistent
representation. This is metadata, and can be part of the response as well as
associated with a specific entity. Metadata is just an arbitrary key/value
JSON structure.

For example::

  {
    "entities" : {
      {
        "name" : "ed",
        "metadata" : {
          "collections" : ["activities", "groups", "followers"]
        }
      }
    },
    "metadata" : {
      "foo" : ["bar", "baz"]
    }
  }

Here's a full example of the response object with one entity in the response
(Note that the facebook property, which contains the entire facebook profile
of the user, is not displayed in the example due to it's size)::

  {
    "action" : "get",
    "application" : "ddde7630-90b1-11e0-b91b-12313f0204bb",
    "params" : { },
    "path" : "/users",
    "uri" : "http://api.usergrid.com/ddde7630-90b1-11e0-b91b-12313f0204bb/users",
    "entities" : [
      {
        "created" : 1307415547108000,
        "facebook" : { ... },
        "uuid" : "1c18ca40-90b2-11e0-b91b-12313f0204bb",
        "metadata" : {
          "path" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb",
          "collections" : {
            "activities" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/activities",
            "feed" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/feed",
            "groups" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/groups",
            "messages" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/messages",
            "queue" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/queue",
            "roles" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/roles",
            "following" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/following",
            "followers" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/followers"
          },
          "sets" : {
            "rolenames" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/rolenames",
            "permissions" : "/users/1c18ca40-90b2-11e0-b91b-12313f0204bb/permissions"
          }
        },
        "modified" : 1307415547108000,
        "name" : "John Doe",
        "picture" : "http://profile.ak.fbcdn.net/hprofile-ak-snc4/41501_217925_2656_q.jpg",
        "type" : "user",
        "username" : "john.doe"
      }
    ],
    "timestamp" : 1309218486419,
    "duration" : 40
  }




