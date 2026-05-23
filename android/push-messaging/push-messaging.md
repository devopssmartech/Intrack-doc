---
title: Push Messaging
version: 3.0.0+
source_path: android/push-messaging/push-messaging.md
docs_url: https://docs.intrack.ir/docs/getting-start/android/push-messaging/push-messaging
---
# Push Messaging

:::warning

Please ensure that you have completed all the platform integration steps listed under [GettingStarted](../android-getting-started) before proceeding.

:::

Push Notifications are messages that pop up on mobile devices. App publishers can send them at any time; even if the recipients aren't currently engaging with the app or using their devices.

You can start sending Push Notifications to your Android users via **inTrack** by configuring [Firebase Cloud Messaging (FCM)](https://firebase.google.com/docs/cloud-messaging/).

Here's how you can go about it:

## 1. Add Firebase to Your Project

Please follow these instructions for [adding Firebase to Your Android Project](https://firebase.google.com/docs/android/setup).

## 2. Add Firebase Cloud Messaging Dependency

Add the dependency for Firebase Cloud Messaging in your module's build.grad.

### Groovy

```groovy
{
`dependencies {
     implementation 'com.google.firebase:firebase-messaging:22.0.0'
}`
}
```

### KotlinDSL

```kotlin
{
`dependencies {
     implementation ("com.google.firebase:firebase-messaging:22.0.0")
}`
}
```

## 3.Initializing notification service

Initializing **inTrack** notification service is done by calling `$inTrack.initPush()`. This method should be called after init method of the **inTrack** SDK:

### Java

```java
{
`$InTrack.initPush(Application application)`
}
```



### kotlin

```kotlin
{
`$InTrack.initPush(Application())`
}
```

## 4. Pass Firebase Token to our Server

Firebase tokens can be passed to **inTrack** using FirebaseMessagingService

### Java

```java
{
`public class MyFirebaseMessagingService extends FirebaseMessagingService {
    @Override
    public void onNewToken(String s) {
      super.onNewToken(s);
      $InTrack.onTokenRefresh(String token)
    }
}`
}
```

### kotlin

```kotlin
{
`class MyFirebaseMessagingService : FirebaseMessagingService() {
    override fun onNewToken(token : String) {
      $InTrack.onTokenRefresh(token)
    }
  }
`
}
```

:::note

Mandatory Step

It is also mandatory that you pass Firebase token to **inTrack** from onCreate of your Application class as shown below. This will ensure that changes in user's Firebase token are communicated to **inTrack**.

:::

### Java

```java
{
`FirebaseMessaging.getInstance().getToken().addOnCompleteListener(
    new OnCompleteListener<String>() {
      @Override
      public void onComplete(@NonNull Task<String> task) {
        if (task.isSuccessful()) {
          $InTrack.onTokenRefresh(token);
        }
      }
    }
);`
}
```

### kotlin

```kotlin
{
`FirebaseMessaging.getInstance().token.addOnCompleteListener {
    task -> if (task.isSuccessful) {
      $InTrack.onTokenRefresh(token)
    }
}`
}
```



## 5. Pass Messages to SDK

Create a class that extends `FirebaseMessagingService` and pass messages to **inTrack**.
If you want **inTrack** handle displaying push notifications you should call **inTrack.displayMessage** method. Before defining messaging service be sure [notification channel](../../android/push-messaging/notification-channels.md) created.

### Java

```java
{
`public class MyFirebaseMessagingService extends FirebaseMessagingService {
    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {
        if (Objects.equals(remoteMessage.getData().get("source"), "$inTrack")) {
            Boolean result = $InTrack.displayMessage(
                applicationContext,
                remoteMessage.getData(),
                "your_push_notification_channel_id",
                R.drawable.ic_launcher_foreground,
                intent
            )
            if (result == null) {
                Log.i(TAG,
                  "Message doesn't have anything to display or wasn't sent from "
                  + "$InTrack, so it cannot be handled by $InTrack SDK");
            } else if (result) {
                Log.i(TAG, "Message was handled by $InTrack SDK");
            } else {
                Log.i(TAG,
                    "Message wasn't handled by $InTrack SDK because API level is too "
                    + "low for Notification support or because currentActivity is null "
                    + "(not enough lifecycle method calls)");
            }
        }
    }
}`
}
```

### kotlin

```kotlin
{
  `class MyFirebaseMessagingService : FirebaseMessagingService() {
    override fun onMessageReceived(remoteMessage : RemoteMessage) {
      val result = $InTrack.displayMessage(
        applicationContext,
        remoteMessage.data,
        getString(R.string.default_notification_channel_id),
        R.mipmap.ic_launcher,
        intent
      )
      if (result == null) {
          Log.i(TAG,
            "Message doesn't have anything to display or wasn't sent from "
            + "CoreProxy, so it cannot be handled by CoreProxy SDK");
      } else if (result) {
          Log.i(TAG, "Message was handled by CoreProxy SDK");
      } else {
        Log.i(TAG,
            "Message wasn't handled by CoreProxy SDK because API level is too "
            + "low for Notification support or because currentActivity is null "
            + "(not enough lifecycle method calls)");
       }
     }
  }`
}
```

## 6. Add Your Firebase Credentials to Our Panel
 **Step 1:** Log in to the [Firebase Developers Console](https://console.firebase.google.com/).

 **Step 2:** Select your Firebase project.

 **Step 3:** Navigate to Project Settings > Cloud Messaging and copy your server key.

 <img title="a title" alt="Alt text" src="/img/firebase_server_key.png"></img>

For existing projects, cloud messaging API is enabled by default.

**For newly created projects, cloud messaging API (Legacy) support is disabled by default**

To enable the support follow the steps below:

* Under "Cloud Messaging API (Legacy)" click on the options (three dots on the right) and click "Manage API in Google Cloud Console".

* You will be redirected to Cloud Messaging API page, where you can enable the API support.

* Post enabling, refresh your Firebase Console page, and now you can see that the Cloud Messaging API (Legacy) has been enabled and Server Key is visible.

**Step 4:** Log in to your [inTrack dashboard](https://dash.intrack.ir) and navigate to Settings > Channels > Push.

* Paste the copied server key under the field labeled FCM SERVER KEY under the Android tab.

* Enter your application package name under the field labeled Package Name and click Save.

:::note

if you want to make sure that message is sent by **inTrack**, you should check source key of message data. note that **inTrack** always send data push message and source key is equal to **inTrack**.

:::

## Making your app compatible with Android 13 push change

From Android 13 onwards, clients will have to explicitly ask permissions from end user to send push notifications. This means, If your app targets Android 13 or higher, and a user installs your app on a device that runs Android 13 or higher, your app's notifications are off by default, so you need to ask users to grant the permissions.

Kindly refer to official [Google documentation](https://developer.android.com/develop/ui/views/notifications/notification-permission) to make your application compatible with the same.

:::info

You can checked push notification system events [here](../../events#push-system-events)

:::

## Timer Count Down push

from v3+ Android sdk supports a new type of push notification layout called "Timer Count Down Push". it can be useful for timed marketing campaigns.

![Timer Count Down Push](../../../../static/img/timer-count-down.png)

this type of push notification requires additional permission to work properly. you should add the following permission to your AndroidManifest.xml file:

```xml
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_SPECIAL_USE" />
```

if the permission is not added, the push notification will not be displayed.

if you will publish your app on Google Play Store, you will be asked about the requirement of this permission. you should explain that this permission is required for displaying Timer Count Down push notifications.
