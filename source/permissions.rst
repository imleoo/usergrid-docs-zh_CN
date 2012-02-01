===========
Permissions
===========

Apache Shiro is used to manage the permissions - http://shiro.apache.org/permissions.html

When a request is made, Usergrid loads all the permissions for the user and all the permissions for all the roles the user is in.

Our permission format extends the default Shiro model as follows::

  <operations>:<entity path pattern>

<operations> is the comma-delimited set of allow operations (get,put,post,delete) and <entity path pattern> is the entity path, which is evaluated using Apache Ant pattern matching (http://ant.apache.org/manual/dirtasks.html#patterns).

Examples:

1) a permission that allows anyone to create a user::

    post:/users

2) a permission that allows someone to look at a specific user::

    get:/users/edanuff

3) a permission that allows the current user to see their activity feed::

    get:/users/${user}/feed

The "${user}" in the permission means "use the current user's id for evaluating this permission"

4) a permission allowing any linked entities to be read::

    get:/users/${user}/**

    which would match things like::

    /users/${user}/feed
    /users/${user}/feed/item1/a/b/c

Property-level Permissions (not-yet implemented)
------------------------------------------------

You can also add property-level permissions to permission rules.  These take the format of::

  <operations>:<entity path patter>:<property path pattern>

<property path pattern> applies a path pattern using Json Pointer (http://tools.ietf.org/html/draft-pbryan-zyp-json-pointer-02).

Examples:

Given the followng JSON entity::

  {
    "type" : "user",
    "username" : "edanuff",
    "foo" : "bar"
  }

1) Allow reading of just the foo property::

    get:/users/edanuff/:/foo

=============
API Endpoints
=============

Please remember to include an access_token with all API calls unless the
documentation explicitly says it's not necessary.

Get the Roles in an Application
-------------------------------

.. http:method:: GET /{app_id}/rolenames

   :arg uuid|string app_id: application UUID or application name
   :Response Body: Example of entity result:

   .. code-block:: js

    {
      "action" : "get",
      "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "params" : {
        "_" : [ "1328058070002" ]
      },
      "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "entities" : [ ],
      "data" : {
        "admin" : "Administrator",
        "default" : "Default",
        "guest" : "Guest"
      },
      "timestamp" : 1328058069799,
      "duration" : 46
    }

   Gets the roles for the specific app.

-------------------
Create A New Role
-------------------

.. http:method:: POST /{app_id}/rolenames

   :arg uuid|string app_id: application UUID or application name
   :Request Body: A JSON object with a rolename and title:

   .. code-block:: js

     { "name" : "manager", "title" : "Manager" }


   :Response Body:  Example of newly created role result:

   .. code-block:: js

    {
      "action" : "get",
      "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "params" : {
        "_" : [ "1328058070002" ]
      },
      "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "entities" : [ ],
      "data" : {
        "admin" : "Administrator",
        "default" : "Default",
        "manager" : "Manager",
        "guest" : "Guest"
      },
      "timestamp" : 1328058069799,
      "duration" : 46
    }

   Creates a new application role.

---------------------------------------
Delete An Role
---------------------------------------

.. http:method:: DELETE /{app_id}/rolenames/{rolename}

   :arg uuid|string app_id: application UUID or application name
   :arg string rolename: a role name
   :Response Body:  Example of deleted entity result:

   .. code-block:: js

    {
      "action" : "get",
      "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "params" : {
        "_" : [ "1328058070002" ]
      },
      "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "entities" : [ ],
      "data" : {
        "admin" : "Administrator",
        "default" : "Default",
        "guest" : "Guest"
      },
      "timestamp" : 1328058069799,
      "duration" : 46
    }

   Deletes the role with the specified rolename.

   Returns the new set of application roles.

-------------------------------------------
Get the Permissions for an Application Role
-------------------------------------------

.. http:method:: GET /{app_id}/rolenames/{rolename}

   :arg uuid|string app_id: application UUID or application name
   :arg string rolename: a role name
   :Response Body: Example of entity result:

   .. code-block:: js

    {
      "action" : "get",
      "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "params" : {
        "_" : [ "1328058543902" ]
      },
      "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "entities" : [ ],
      "data" : [
        "get,put,post,delete:/users/${user}",
        "get,put,post,delete:/users/${user}/activities",
        "get,put,post,delete:/users/${user}/feed",
        "get,put,post,delete:/users/${user}/following/*",
        "get,put,post,delete:/users/${user}/following/user/*",
        "get,put,post,delete:/users/${user}/groups"
      ],
      "timestamp" : 1328058543530,
      "duration" : 33
    }

   Gets the permissions for the specific app role.

-------------------------------------------
Add a Permissions to an Application Role
-------------------------------------------

.. http:method:: POST /{app_id}/rolenames/{rolename}

   :arg uuid|string app_id: application UUID or application name
   :arg string rolename: a role name
   :Request Body: A JSON object with a rolename and title:

   .. code-block:: js

     { "permission" : "get,put,post,delete:/users/${user}/groups" }


   :Response Body: Example of entity result:

   .. code-block:: js

    {
      "action" : "get",
      "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "params" : {
        "_" : [ "1328058543902" ]
      },
      "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "entities" : [ ],
      "data" : [
        "get,put,post,delete:/users/${user}",
        "get,put,post,delete:/users/${user}/activities",
        "get,put,post,delete:/users/${user}/feed",
        "get,put,post,delete:/users/${user}/following/*",
        "get,put,post,delete:/users/${user}/following/user/*",
        "get,put,post,delete:/users/${user}/groups"
      ],
      "timestamp" : 1328058543530,
      "duration" : 33
    }

   Gets the permissions for the specific app role.

---------------------------------------------
Remove a Permissions from an Application Role
---------------------------------------------

.. http:method:: DELETE /{app_id}/rolenames/{rolename}?permission={permission}

   :arg uuid|string app_id: application UUID or application name
   :arg string rolename: a role name
   :arg string permission: a permission
   :Response Body: Example of entity result:

   .. code-block:: js

    {
      "action" : "get",
      "application" : "7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "params" : {
        "_" : [ "1328058543902" ]
      },
      "uri" : "http://api.usergrid.com/7fb8d891-477d-11e1-b2bd-22000a1c4e22",
      "entities" : [ ],
      "data" : [
        "get,put,post,delete:/users/${user}",
        "get,put,post,delete:/users/${user}/activities",
        "get,put,post,delete:/users/${user}/feed",
        "get,put,post,delete:/users/${user}/following/*",
        "get,put,post,delete:/users/${user}/following/user/*",
      ],
      "timestamp" : 1328058543530,
      "duration" : 33
    }

   Removes the permissions for the specific app role.



