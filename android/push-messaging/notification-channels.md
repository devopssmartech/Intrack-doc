---
title: Notification Channels
version: 3.0.0+
source_path: android/push-messaging/notification-channels.md
docs_url: https://docs.intrack.ir/docs/getting-start/android/push-messaging/notification-channels
---
# Notification Channels

From Android 8.0 (API level 26) and higher, [notification channels](https://developer.android.com/develop/ui/views/notifications#ManageChannels) are supported and recommended. If you prefer to create and use your own default channel, set `default_notification_channel_id` to the ID of your notification channel . Notification channels provide a unified system to help you manage notification behaviour depending on their type.

You can modify default push channel configurations by calling specific methods of NotificationManager class before SDK initialization as shown below.

### Java

```java
{
`String channelId = getString(R.string.default_notification_channel_id);
String channelName = getString(R.string.default_notification_channel_name);
NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
if (notificationManager != null) {
      NotificationChannel channel = new NotificationChannel(channelId, channelName, NotificationManager
      IMPORTANCE_DEFAULT);
      notificationManager.createNotificationChannel(channel);
}`
}
```

### Kotlin

```kotlin
{
`val channelId = getString(R.string.default_notification_channel_id)
val channelName = getString(R.string.default_notification_channel_name)
val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager?
notificationManager?.let {
      val channel = NotificationChannel(channelId, channelName, NotificationManager.IMPORTANCE_DEFAULT)
      notificationManager.createNotificationChannel(channel)
}`
}
```

**ID**

This sets the ID for the particular channel. If a channel with the ID is already present, the existing channel will be used and the values for the channel will not be overridden. If channel is not present with the channel ID provided, a new channel will be created with the configured values.

**Name**

This is the channel name which is visible to the user.

**Importance**

Sets one of five importance levels that configure the amount a channel can interrupt a user, ranging from `IMPORTANCE_NONE (0)` to `IMPORTANCE_HIGH (4)`. The default importance level is 3 which displays everywhere, makes noise, but doesn't visually intrude on the user.

:::note

Note that for displaying push notification when your app is in a background, you need to register android push notification channel with *High importance*, and you need to send the ID (your_push_notification_channel_id in above sample code) of that channel to the above method.

:::
