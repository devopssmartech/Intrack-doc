---
title: In-app Messaging
version: 3.0.0+
source_path: flutter/inapp-messaging.md
docs_url: https://docs.intrack.ir/docs/getting-start/flutter/inapp-messaging
---
# In-app Messaging

## Initialization

you can enable In App messaging service by calling **enableInAppMessaging** on config object during **inTrack** initialization.

:::warning

for using In App messaging you should use **inTrack** sdk (version 2 or higher)

:::

For Android platform, you should make sure that your Android's `MainActivity` extends `FlutterFragmentActivity` or any other activity that have set
[`ViewTreeLifeCycleOwner`](https://developer.android.com/reference/androidx/lifecycle/ViewTreeLifecycleOwner) on its [`decorView`](https://developer.android.com/reference/android/view/Window#getDecorView())

### Java

```java
{
`import io.flutter.embedding.android.FlutterFragmentActivity;

public class MainActivity extends FlutterFragmentActivity {
}`
}
```

### Kotlin

```kotlin
{
`import io.flutter.embedding.android.FlutterFragmentActivity

public class MainActivity : FlutterFragmentActivity {
}`
}
```

then in your flutter code you can initialize the feature:

```dart
{
`$InTrack.isInitialized().then((bool isInitialized) {
    if (!isInitialized) {
      $InTrackConfig config =
          $InTrackConfig("YOUR_APP_KEY")
            ..setAndroidAuthKey("YOUR_ANDROID_AUTH_KEY")
            ..setIOSAuthKey("YOUR_IOS_AUTH_KEY")
            ..enableInAppMessaging(); //<--HERE
      $InTrack.initWithConfig(config).then((value) {
        print('$inTrack $value');
        //you can enable push notification here
      });
    }
});
`
}
```

:::note

At present, In App Messaging functionality is exclusively available for Android devices.

:::

## Custom Screen tracking

Screens are mobile equivalent of web pages, which can have associated properties. **inTrack** SDK allows you to tag whenever a user sees a screen.These tags allow you to pinpoint screens in your app where you would later render in-app messages using [inTrack dashboard](https://dash.intrack.ir). Note that screens are only usable for targeting in-app engagements.
you can manually tell **inTrack** about user navigation by calling inTrack's recordNavigation method:

```dart
{
  `$InTrack.recordNavigation({
        'screenName': 'YOU_SCREEN_NAME'
    });
`
}
```

## Tracking Screen Data

Every screen can be associated with some contextual data, which can be part of the targeting rule for in-app messages. You can set the screen name and data as shown below:

```dart
{
  `$InTrack.recordNavigation({
        'screenName':  'YOU_SCREEN_NAME', -> mandatory
        'screenData': {'key': 'value'}
     });
`
}
```

:::note

currently we don't have any general solution for automatic screen tracking.

:::

marketing agents can run campaigns based on number of pages that a user navigates through you app, so each time you call recordNavigation we will increase user's page view count.

:::info

You can checked InApp system events [here](../events#inapp-system-events)

:::
