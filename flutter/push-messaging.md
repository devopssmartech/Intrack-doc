---
title: Push Messaging
version: 3.0.0+
source_path: flutter/push-messaging.md
docs_url: https://docs.intrack.ir/docs/getting-start/flutter/push-messaging
---
# Push Messaging

:::note

Before continuing, please ensure that you've [ added the Flutter SDK to your app ](../flutter/flutter-getting-started.md).

:::

## For Android

Push Notifications are messages that pop up on mobile devices. App publishers can send them at any time; even if the recipients aren't currently engaging with the app or using their devices.

### Integrating with your Flutter Android App (FCM not integrated)

You can start sending Push Notifications to your Android users via **inTrack** by configuring Firebase Cloud Messaging (FCM).
Here's how you can enable Push Messaging for your Android app via the **inTrack** Flutter SDK.

**Step 1:** Add Firebase to Your Project by following the necessary steps in [FCM Docs](https://firebase.google.com/docs/android/setup)

**Step 2:** Add the below dependencies in the app-level build.gradle file.

```groovy
{
    `implementation platform('com.google.firebase:firebase-bom:25.12.0')
    implementation 'com.google.firebase:firebase-analytics'
    implementation 'com.google.firebase:firebase-messaging:20.2.1'
`
}
```

**Step 3:** Add the following to your dependencies section in project/build.gradle file.

```groovy
{
    `classpath 'com.google.gms:google-services:4.3.4'
`
}
```

**Step 4:** Add the following line in file `android/app/src/main/AndroidManifest.xml` inside application tag.

```xml
{
    ` <service
        android:name="ir.$intrack.flutter.sdk.$InTrackMessagingService"
        android:exported="false">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
</service>
        <meta-data
            android:name="ir.$intrack.sdk.default_notification_channel_id"
            android:value="YOUR_CHANNEL_ID" />
        <meta-data
            android:name="ir.$intrack.sdk.default_notification_channel_name"
            android:value="YOUR CHANNEL NAME" />
        <meta-data
            android:name="ir.$intrack.sdk.default_notification_channel_description"
            android:value="YOUR CHANNEL DESCRIPTION" />
`
}
```

**channelName:** This is the human-readable name of the notification channel that will be displayed to users in the system settings.

**channelId:** This is a unique identifier for the notification channel. It's used internally by the system to manage and deliver notifications to the correct channel.

**channelDescription:** This provides a brief description or explanation of the purpose of the notification channel. It helps users understand the type of notifications they will receive through this channel.

:::note

inTrack will create a push notification channel for you, you can change channel properties by providing meta-data value.

:::

**step 5:** Initialize push notification service, for initiating push notification service you just neet to call
`$InTrack.askForNotificationPermission()` function, after initializing the SDK itself.

```dart
{
    `$InTrack.isInitialized().then((bool isInitialized) {
        if (!isInitialized) {
            $InTrackConfig config = $InTrackConfig(APP_KEY)
            ..setAndroidAuthKey(ANDROID_AUTH_KEY)
            ..setIOSAuthKey(IOS_AUTH_KEY)

            config.setLoggingEnabled(true);
            $InTrack.initWithConfig(config).then((value) {

            // This method will ask for permission, enables push notification channel and send push token to $inTrack server.;
            $InTrack.askForNotificationPermission();
            }); // Initialize the $InTrack SDK.
        }
    });
`
}
```

### Handling multiple FCM services

**inTrack** push notifications function independently, but issues may arise if your project includes other messaging services (such as additional push notification plugins or dependencies). This can sometimes interfere with inTrack's push notifications.

To support multiple messaging services, you should create a wrapper messaging service and register it in your `AndroidManifest.xml` file:

```xml
{
    `<service android:name=".WrapperMessagingService" android:exported="false">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT" />
         </intent-filter>
</service>
`
}
```

Inside `WrapperMessagingService.java`, you'll need to forward messages to other messaging services.

In the code below, we assume you are using `FlutterFirebaseMessagingService`. If you are using additional `FirebaseMessagingServices` from other packages or dependencies that you have imported, you will need to include them as well.

```swift
{
    `package YOUR.APP.PACKAGE.NAME;
    import java.lang.reflect.Field;
    import java.util.ArrayList;
    import java.util.List;

    import com.google.firebase.messaging.FirebaseMessagingService;
    import com.google.firebase.messaging.RemoteMessage;

    import io.flutter.plugins.firebase.messaging.FlutterFirebaseMessagingService;
    import ir.$intrack.flutter.sdk.$InTrackMessagingService;

    public class WrapperMessagingService extends FirebaseMessagingService{
        private List<FirebaseMessagingService> messagingServices = new ArrayList<>(2);

        public WrapperMessagingService() {
            messagingServices.add(new $InTrackMessagingService());
            messagingServices.add(new FlutterFirebaseMessagingService());
        }

        private void addService(FirebaseMessagingService firebaseMessagingService) {
            injectContext(firebaseMessagingService);
            messagingServices.add(firebaseMessagingService);
        }

        @Override
        public void onNewToken(String s) {
            delegate(service -> {
                injectContext(service);
                service.onNewToken(s);
            });
        }

        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
            delegate(service -> {
                injectContext(service);
                service.onMessageReceived(remoteMessage);
            });
        }

        private void delegate(GCAction1<FirebaseMessagingService> action) {
            for (FirebaseMessagingService service : messagingServices) {
                action.run(service);
            }
        }

        private void injectContext(FirebaseMessagingService service) {
            // Accessing hidden field Landroid/content/ContextWrapper;->mBase:Landroid/content/Context; (greylist, reflection)
            //
            // https://developer.android.com/distribute/best-practices/develop/restrictions-non-sdk-interfaces
            // https://android.googlesource.com/platform/frameworks/base/+/pie-release/config/hiddenapi-light-greylist.txt
            setField(service, "mBase", this);
        }

        private boolean setField(Object targetObject, String fieldName, Object fieldValue) {
            Field field;
            try {
                field = targetObject.getClass().getDeclaredField(fieldName);
            } catch (NoSuchFieldException e) {
                field = null;
            }
            Class superClass = targetObject.getClass().getSuperclass();
            while (field == null && superClass != null) {
                try {
                    field = superClass.getDeclaredField(fieldName);
                } catch (NoSuchFieldException e) {
                    superClass = superClass.getSuperclass();
                }
            }
            if (field == null) {
                return false;
            }
            field.setAccessible(true);
            try {
                field.set(targetObject, fieldValue);
                return true;
            } catch (IllegalAccessException e) {
                return false;
            }
        }

        interface GCAction1<T> {
            void run(T t);
        }
    }
`
}
```

### Push Notification Callback

o receive notifications when a user clicks on a push notification sent by inTrack, you can register a callback function using the onNotification method:

```dart
{
    `$InTrack.onNotification((String notification) {
        print('The notification');
        print(notification);
    });
`
}
```

### Manual setup

We recommend using the above settings to set up inTrack push notifications. However, if you prefer to manually handle push notifications in your Flutter application (using packages like FlutterFire), you will need to implement the following steps yourself.

* Pass Firebase tokens to **inTrack**:

```dart
{
    `$InTrack.onTokenRefresh(fcmToken)
`
}
```

* Pass messages to **inTrack**:

```dart
{
    `Map<String, dynamic> data = message.data;
    if (data.isNotEmpty && data['source'] == "$inTrack") {
          $InTrack.displayMessage(data, 'HIGH_IMPORTANCE_CHANNEL_ID');
        }
`
}
```

:::note

To displaying push notification when your app is in a background, you need to register android push notification channel with High importance.

:::

:::info

You can checked push notification system events [here](../events#push-system-events)

:::

### Timer Count Down push

for this section please refer to the section [Timer Count Down Push Messaging](../android/push-messaging/push-messaging.md#timer-count-down-push) in android push notification setup.
