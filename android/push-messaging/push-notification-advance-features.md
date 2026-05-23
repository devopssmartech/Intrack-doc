---
title: Push notification advance features
version: 3.0.0+
source_path: android/push-messaging/push-notification-advance-features.md
docs_url: https://docs.intrack.ir/docs/getting-start/android/push-messaging/push-notification-advance-features
---
# Push notification advance features

The **inTrack** SDK provides a helper class named InTrackPush designed to offer enhanced control and flexibility in handling push notifications. Below are some common scenarios:

## Decoding Incoming Messages
You can utilize the **InTrackPush.decodeMessage** method to extract message data, especially custom data or key-value pairs defined in the [inTrack dashboard](https://dash.intrack.ir):

### Java

```java
{
`$InTrackPush.Message message = $InTrackPush.decodeMessage(remoteMessage.getData());
if (message == null) {
    Log.d(TAG, "The message was not sent from $InTrack");
    return;
}
if (message.customData().containsKey("your-custom-key")) {
    // Perform actions with your custom key-value data
}`
}
```

### kotlin

```kotlin
{
`val message = $InTrackPush.decodeMessage(remoteMessage.data)
if (message == null) {
    Log.d(TAG, "The message was not sent from $InTrack")
    return
}
if (message.customData().containsKey("your-custom-key")) {
    // Perform actions with your custom key-value data
}`
}
```

## Push Message Handling

To enable custom processing of push notifications, you may register a BroadcastReceiver:

### Java

```java
{
`BroadcastReceiver messageReceiver = new BroadcastReceiver() {
    @Override
    public void onReceive(Context context, Intent intent) {
      Intent sentIntent = intent.getParcelableExtra($InTrackPush.EXTRA_INTENT);
      sentIntent.setExtrasClassLoader($InTrackPush.class.getClassLoader());
      Bundle bun = sentIntent.getParcelableExtra($InTrackPush.EXTRA_MESSAGE);
      $InTrackPush.Message message;
      int actionIndex =
          sentIntent.getIntExtra($InTrackPush.EXTRA_ACTION_INDEX, -100);
      String title = "NULL";
      if (bun != null) {
        message = bun.getParcelable($InTrackPush.EXTRA_MESSAGE);
        if (message != null) {
          title = message.title();
        }
      }
      Log.i("$InTrack", "Received message with title: [" + title + "]");
    }
  };
  IntentFilter filter = new IntentFilter();
  filter.addAction($InTrackPush.SECURE_NOTIFICATION_BROADCAST);
  registerReceiver(messageReceiver, filter, getPackageName() + $InTrackPush.$INTRACK_BROADCAST_PERMISSION_POSTFIX, null);
`
}
```

### kotlin

```kotlin
{
`// Overriding Push Message Handling
val messageReceiver = object : BroadcastReceiver() {
    override fun onReceive(context : Context, intent : Intent) {
      val sentIntent = intent.getParcelableExtra<Intent>($InTrackPush.EXTRA_INTENT)
      sentIntent?.apply {
        setExtrasClassLoader($InTrackPush::class.java.classLoader)
        val bun = getParcelableExtra<Bundle>($InTrackPush.EXTRA_MESSAGE)
        val message : $InTrackPush.Message?
        val actionIndex = getIntExtra($InTrackPush.EXTRA_ACTION_INDEX, -100)
        var title = "NULL"
        bun?.apply {
            message = getParcelable($InTrackPush.EXTRA_MESSAGE)
            message?.apply {
                title = title()
            }
        }
        Log.i("$InTrack", "Received message with title: [$title]")
      }
    }
}
val filter = IntentFilter().apply{
    addAction($InTrackPush.SECURE_NOTIFICATION_BROADCAST)
}
registerReceiver(messageReceiver, filter, packageName + $InTrackPush.$INTRACK_BROADCAST_PERMISSION_POSTFIX, null)`
}
```



**Note:** In InTrack.displayMessage, you can pass an activity-starting intent to be launched when the user taps on the notification. If you pass null, the main activity will be launched. Similarly, you can retrieve message data from your activity's intent:

### Java

```java
{
`// Retrieving data from intent
Bundle bun = intent.getParcelableExtra($InTrack.EXTRA_MESSAGE);
$InTrackPush.Message message;
String title = "NULL";
if (bun != null) {
      message = bun.getParcelable($InTrackPush.EXTRA_MESSAGE);
      if (message != null) {
          title = message.title();
      }
}`
}
```

### kotlin

```kotlin
{
`val bun = intent.getParcelableExtra<Bundle>($InTrack.EXTRA_MESSAGE)
val message: $InTrackPush.Message?
var title = "NULL"
bun?.apply {
    message = getParcelable($InTrackPush.EXTRA_MESSAGE)
    message?.apply {
      title = title()
    }
  }`
}
```
