=====================================
Service Gateways & Push Notifications
=====================================

Subscriptions are also useful for distributing messages to special gateway
queues for relaying to other message-oriented services such as Twitter or
Facebook, or the Apple Push Notification Service and Android Cloud to Device
Messaging. For example, to relay messages to an iOS device, you would add the
following queue as a subscriber to your user's inbox queue:
 
  /apns/*<ios_device_id>*
 
For Android applications, the subscriber would be:
 
  /c2dm/*<android_registration_id>*
 
For relaying to Twitter, the subscriber would be:
 
  /twitter/*<twitter_user_id>*
 
