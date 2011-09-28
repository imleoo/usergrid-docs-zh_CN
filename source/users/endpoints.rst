=============
API Endpoints
=============

-------------------
Set A User Password
-------------------

.. http:method:: POST /{app_id}/users/{user}/password

   :arg uuid|string app_id: application UUID or application name
   :arg string user: a user email or username
   :Request Body: The new and old passwords

   .. code-block:: js

     { "newpassword" : "foo", "oldpassword" : "bar" }

  A password must be set when the user is first created. If accessing the
  endpoint with Application-level access, as opposed Application User-level
  access or anonymous access, then the old password does not need to be
  provided.