
==============================
Authentication & Authorization
==============================

Usergrid is authenticated via oAuth 2.0. Unlike oAuth 1.0, which required
special support in the client code for signing requests, oAuth 2.0 can be used
by any web service client libraries. Although, at the time of this writing,
the oAuth 2 specification is not finalized, it's sufficiently far along that
many web service providers, including Google and Facebook, have switched to
using it for their authentication mechanism. You can find information about
oAuth 2 at `Oauth.net <http://oauth.net/2/>`_.

Usergrid allows four types of access:

=================  ==============================================================================================================
Access Type        Description
=================  ==============================================================================================================
Organization       full access to perform any operation on a Usergrid organization account
Admin User         full access to perform any operation on all Usergrid organization accounts the admin user is a member of
Application        full access to perform any operation on a Usergrid application
Application User   policy-limited access to perform operations on a Usergrid application
=================  ==============================================================================================================

Usergrid requires that you use the standard oAuth mechanism of obtaining
an access token and then passing that token along with your requests.  To get
the access token, you'll connect to the appropriate web service endpoint and provide
the correct client credentials.

Organization and Application access are meant to be used by your server-side
applications. They are "superuser" access mechanisms and have little
constraints on what they can do. When connecting via oAuth, you'll use the
client_id and client_secret parameters as your client credetials.

Admin User and Application User access involved situations where you're
connecting as a known user account, either as an admin user who may be a
member of several organizations and have management rights to several apps, or
as an application user who belongs to a single app. When connecting via oAuth,
you'll use the username and password parameters as your client credetials.

The following sections will describe the specifics of each of these levels of
access and provide examples of getting an access token.

-------------
Organizations
-------------

When you sign up for Usergrid, you create an Organization account in addition
to your personal Admin User account. Organizations can be accessed with
client_id/client_secret credentials to operate on a single Organization or by
an Admin User who provides username/password credentials.

Using your client_id and client_secret values (obtained from the Admin
Console), you'd connect to the following URL (substituting the correct values
for <client_id> and <client_secret>)::

  http://api.usergrid.com/management/management/token?grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>

The results would look something like this::

    {
      "access_token": "gAuFEOlXEeCfRgBQVsAACA:b3U6AAABMqz-Cn0wtDxxkxmQLgZvTMubcP20FulCZQ",
      "expires_in": 3600,
      "organization": {
        "users": {
          "test": {
            "name": "Test User",
            "disabled": false,
            "uuid": "7ff1946d-e957-11e0-9f46-005056c00008",
            "activated": true,
            "username": "test",
            "applicationId": "00000000-0000-0000-0000-000000000001",
            "email": "test@usergrid.com",
            "adminUser": true,
            "mailTo": "Test User <test@usergrid.com>"
          }
        },
        "name": "test-organization",
        "applications": {
          "test-app": "8041893b-e957-11e0-9f46-005056c00008"
        },
        "uuid": "800b8510-e957-11e0-9f46-005056c00008"
      }
    }

The above results provide the access_token value needed to make subsequent API requests
as well as some additional information about the organization.

-----------
Admin Users
-----------

Admin Users are users of the Usergrid.com service and can be members of one or
more Organizations. An Organization, in turn, can have one or more Admin
Users. Currently, all Admin Users in an Organization have full access
permissions. In the future, Admin Users will have configurable levels of
Organization account access permissions on a per-Organization basis. Admin
Users are authenticated with Basic credentials.

Using your username and password values (specified when you created your admin
account), you'd connect to the following URL (substituting the correct values
for <username> and <password>)::

  http://api.usergrid.com/management/management/token?grant_type=password&username=<username>&password=<password>

The results would look something like this::

    {
      "access_token": "f_GUbelXEeCfRgBQVsAACA:YWQ6AAABMqz_xUyYeErOkKjnzN7YQXXlpgmL69fvaA",
      "expires_in": 3600,
      "user": {
        "username": "test",
        "email": "test@usergrid.com",
        "organizations": {
          "test-organization": {
            "users": {
              "test": {
                "name": "Test User",
                "disabled": false,
                "uuid": "7ff1946d-e957-11e0-9f46-005056c00008",
                "activated": true,
                "username": "test",
                "applicationId": "00000000-0000-0000-0000-000000000001",
                "email": "test@usergrid.com",
                "adminUser": true,
                "mailTo": "Test User <test@usergrid.com>"
              }
            },
            "name": "test-organization",
            "applications": {
              "test-app": "8041893b-e957-11e0-9f46-005056c00008"
            },
            "uuid": "800b8510-e957-11e0-9f46-005056c00008"
          }
        },
        "adminUser": true,
        "activated": true,
        "name": "Test User",
        "mailTo": "Test User <test@usergrid.com>",
        "applicationId": "00000000-0000-0000-0000-000000000001",
        "uuid": "7ff1946d-e957-11e0-9f46-005056c00008",
        "disabled": false
      }
    }

------------
Applications
------------

Applications can be accessed with Application oAuth credentials, or with the
Basic credentials of an Admin associated with the Organization that owns the
application, or with the oAuth credentials of the Organization that owns the
application.

Using your client_id and client_secret values (obtained from the Application
Settings section of the Admin Console), you'd connect to the following URL
(substituting the correct values for <app-name>, <client_id> and
<client_secret>)::

  http://api.usergrid.com/<app-name>/token?grant_type=client_credentials&client_id=<client_id>&client_secret=<client_secret>

The results would look something like this::

    {
      "access_token": "F8zeMOlcEeCUBwBQVsAACA:YXA6AAABMq0d4Mep_UgbZA0-sOJRe5yWlkq7JrDCkA",
      "expires_in": 3600,
      "application": {
        "name": "test-app",
        "id": "17ccde30-e95c-11e0-9407-005056c00008"
      }
    }

-----------------
Application Users
-----------------

Application users are the members of the "users" collection within a
application. They are the actual users of your app and are completely separate
from any other application or from the Usergrid.com service. They can
authenticate with either Basic credentials or oAuth credentials. Their access
to the system is controlled by their assigned permissions as well as the roles
they're members of and the permissions assigned to those roles.

Using the username and password values (specified when the app user was
created), you'd connect to the following URL (substituting the correct values
for <app-name>, <username> and <password>)::

  http://api.usergrid.com/management/<app-name>/token?grant_type=password&username=<username>&password=<password>

The results would look something like this::

    {
      "access_token": "5wuGd-lcEeCUBwBQVsAACA:F8zeMOlcEeCUBwBQVsAACA:YXU6AAABMq0hdy4Lh0ewmmnOWOR-DaepCrpWx9oPmw",
      "expires_in": 3600,
      "user": {
        "uuid": "e70b8677-e95c-11e0-9407-005056c00008",
        "type": "user",
        "username": "edanuff",
        "email": "ed@anuff.com",
        "activated": true,
        "created": 1317164604367013,
        "modified": 1317164604367013
      }
    }


----------------------
Using The Access Token
----------------------

Once an access token is obtained, it must be provided with every subsequent
API call you make. It can either be added to the API querystring::

  http://api.usergrid.com/<app-name>/users?access_token=<access_token>

Or provided as an HTTP authorization header in the form of::

  Authorization: Bearer <access_token>

Please note that this documentation assumes you are providing a valid access
token with every API call, whether or not it explicitly mentions it. Unless
the documentation specifically says that an API endpoint can be accessed
without an access token, you should assume you need to provide it.

------------------
Safe Mobile Access
------------------

For mobile access, we only recommend connecting as a Application User and with
configured access control policies. Mobile applications are inherently
untrusted, they can be easily examined and even decompiled and any credentials
stored in a mobile app should be considered only secure to the level of the
app's user. This means that if you wouldn't want your user to be able to
access or delete certain data in your Usergrid application, you need to make
sure that you don't enable that via roles or permissions. Most webapps talk to
the database with root or some level of elevated superuser permissions and it's
generally a good idea for mobile apps to connect with a more restricted set of
permissions.


