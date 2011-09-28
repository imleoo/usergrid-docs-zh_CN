
=====
User
=====

Users have the following properties:

============  =========  =========================================================
Property      Type       Description
============  =========  =========================================================
uuid          UUID       the user's unique entity id
type          string     "user"
created       long       unix timestamp of entity creation
modified      long       unix timestamp of entity modification
username      string     a valid string username
email         string     a valid email address
password      string     user password (encrypted)
activated     boolean    whether user account has been activated
disabled      boolean    whether user account has been administratively disabled
firstname     string     user first name
middlename    string     user middle name
lastname      string     user last name
============  =========  =========================================================

Users have the following set properties:

============  =========  =========================================================
Set           Type       Description
============  =========  =========================================================
connections   string     set of connection types (i.e. "likes", etc.)
rolenames     string     set of roles a user belongs to
permissions   string     set of user permissions
============  =========  =========================================================

Users have the following collections:

============  =========  =========================================================
Collection    Type       Description
============  =========  =========================================================
groups        group      collection of groups a user belongs to
messages      message    collection of messages a user has sent
queue         message    collection of messages a user has received
activities    activity   collection of activities a user has performed
feed          activity   inbox of activity notifications a user has received
roles         role       set of roles a user belongs to
============  =========  =========================================================

