
===========
Data Model
===========

-----------
Entities
-----------

Usergrid calls the basic objects of your application "entities". These
represent things like users as well as content or other application data.

Usergrid pre-defines a set of entity types. These are currently:

* user
* group
* activity
* message
* asset
* folder
* event

Each entity type is stored in it's own collection, so:

* /users
* /groups
* /activities
* /messages
* /assets
* /folders
* /events

You can also create any type of dynamic (custom) entity you want. Dynamic
entities are automatically stored in a collection who's name is the pluralized
form of the entity type. For example, creating an entity of type "cat" will
store the entity in a collection named "cats". As a consequence, please choose
entity types that are singular nouns.

Instances of an entity all have a unique uuid assigned to them. You can assign
your own id property to an entity, but the uuid is the canonical, and fastest,
was to access an entity.

All entities have pre-defined properties as well as have the ability to have
any number of dynamic (custom) properties used with them. The pre-defined
properties may be of specific types for validation purposes, while dynamic
properties can be any thing that is represented by JSON.

All entities have the following properties:

=============  ===========  =========================================
Property       Type         Description
=============  ===========  =========================================
uuid           UUID         Entity unique id
type           string       entity type (i.e. "user")
created        long         unix timestamp of entity creation
modified       long         unix timestamp of entity modification
=============  ===========  =========================================

Dynamic entities also have an optional "name" property that's a string value.

Please refer to the Entity reference in the appendix for the schemas and
business logic associated with the pre-defined entities.

--------------
Collections
--------------

As mentioned previously, all entities are contained in "root" collections. In
addition to being members of the root collections, entities can also be
members of collections held by other entities. For example, every group
maintains a collection of users. Entities are always members of their root
collection even if they're also members of a collection owned by an entity.

Only pre-defined entities own collections, and all entities in a collection
are of the same type. Entity collections are indexed, so you can, for example,
query for all the users in a group who's biography property contains the word
"California".

The collections defined for entities are listed in the Entity section of the
appendix.

--------------
Connections
--------------

Any two entities can be connected together by dynamically creating a
connection between them. These connections are defined with a connection type,
which is an arbitrary string. For example, suppose you want to have a user
"like" a restaurant, you'd create a "like" connection between the user entity
and the restaurant entity. Once you've done this, you can now search across
connections, such as finding all restaurants the user likes that serve Indian
food.
