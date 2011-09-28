
=====
Group
=====

Groups have the following properties:

============  =========  =========================================================
Property      Type       Description
============  =========  =========================================================
uuid          UUID       the user's unique entity id
type          string     "user"
created       long       unix timestamp of entity creation
modified      long       unix timestamp of entity modification
path          string     a valid slash-delimited group path
title         string     a display name
============  =========  =========================================================

Users have the following set properties:

============  =========  =========================================================
Set           Type       Description
============  =========  =========================================================
connections   string     set of connection types (i.e. "likes", etc.)
rolenames     string     set of roles a user belongs to
permissions   string     set of user permissions
============  =========  =========================================================

Groups have the following collections:

============  =========  =========================================================
Collection    Type       Description
============  =========  =========================================================
users         user       collection of users in the group
messages      message    collection of messages a user has sent
queue         message    collection of messages a user has received
activities    activity   collection of activities a user has performed
feed          activity   inbox of activity notifications a user has received
roles         role       set of roles a user belongs to
============  =========  =========================================================

