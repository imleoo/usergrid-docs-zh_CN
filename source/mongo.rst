
=========
Mongo API
=========

Usergrid provides a partial implementation of Mongo the Mongo API so that Mongo clients can connect to Usergrid.

There are a number of drivers and tools for talking to MongoDB, which has a data model that
is very similar to that of Usergrid.  In order to make it easier for people to get started
quickly with Usergrid, we provide an implementation of the Mongo native wire protocol and
to map the key query and CRUD operations to Usergrid equivalents.

------------------------------------------------
Mapping Usergrid Multi-tenancy to Mongo Security
------------------------------------------------

First steps are to simply be able to handle a client login and map it to a
Usergrid account.

Authentication by a Mongo client is described at `Mongo Security and Authentication`_ and `Mongo Implementing Authentication in a Driver`_.

.. _Mongo Security and Authentication: http://www.mongodb.org/display/DOCS/Security+and+Authentication
.. _Mongo Implementing Authentication in a Driver: http://www.mongodb.org/display/DOCS/Implementing+Authentication+in+a+Driver

A user who authenticates against the Mongo 'admin' account will be actually
logging in as a Usergrid admin user who is a member of one or more Usergrid
accounts, each of which contains a set of applications that user is able to
administer. While they list the databases via the Mongo API, what they will be
seeing is the aggregate list of the applications for all the accounts they are
members of.

For any database (Usergrid application), they'll be able to list the collections.

Note: Internally, Mongo refers to it's collections as "namespaces", which is a
potential point of confusion between Usergrid which uses the term "collection"
as it's equivalent to a "database".

--------------------
Implementation Notes
--------------------

See the `Mongo Wire Protocol`_ for information on how native Mongo clients talk to the server.

.. _Mongo Wire Protocol: http://www.mongodb.org/display/DOCS/Mongo+Wire+Protocol

Mongo uses BSON, which is a binary JSON format, as part of the wire protocol.
For compatibility, we're using the BSON encoder from the `Mongo Java Driver`_.
Future versions of Usergrid will switch to using `Bson4Jackson`_.

.. _Mongo Java Driver: https://github.com/mongodb/mongo-java-driver/
.. _Bson4Jackson: https://github.com/michel-kraemer/bson4jackson

See `Mongo Database Commands`_ for a list of commands that Mongo supports.

.. _Mongo Database Commands: http://www.mongodb.org/display/DOCS/List+of+Database+Commands

See `Mongo System Metadata`_ for a list of metadata information a Mongo server
provides.

.. _Mongo System Metadata: http://www.mongodb.org/display/DOCS/Mongo+Metadata

See `Mongo Query Language`_ for the syntax for querying the Mongo database.

.. _Mongo Query Language: http://www.mongodb.org/display/DOCS/Mongo+Query+Language


