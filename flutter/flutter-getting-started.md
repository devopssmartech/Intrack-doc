---
title: Getting Started
version: 3.0.0+
source_path: flutter/flutter-getting-started.md
docs_url: https://docs.intrack.ir/docs/getting-start/flutter/flutter-getting-started
---
# Getting Started

Flutter is an open-source UI software development kit created by Google. It enabled you to develop apps for Android, iOS, Linux, Mac, Windows, and the web from a single codebase. The **inTrack** Flutter Mobile SDK supports all the latest OS versions of Android and iOS. Currently, it is only available for Android and iOS; the Web is not supported.

Here's how you can integrate the **inTrack**  SDK with your Flutter apps:

## 1. Installation

**PreRequirement:**
* minimum Android version of 4.4 (API Level 19)

### Adding dependency

To install the **inTrack** Flutter SDK, use the following command:

```sh
{
`flutter pub add $intrack_flutter`
}
```

### Proguard Rule

If you are using Proguard, add the following rule to your Proguard settings to ensure proper functionality:

```json
{
  `-keep class com.google.android.gms.ads.**{*;}
  -dontwarn io.reactivex.**
  -keep class io.reactivex.** { *; }
  -keep class ir.intrack.android.sdk.** { *; }
  -keep class sdk.main.core.** { *; }
`
}
```

## 2. Initialization

First, import **inTrack** using the following lines:

```dart
{
`import 'package:$intrack_flutter/$intrack_flutter.dart';

}
```

 Initialize **inTrack** in main.dart in initState();

```dart
{
  `$InTrack.isInitialized().then((bool isInitialized) {
      if (!isInitialized) {
        $InTrackConfig config =
            $InTrackConfig("YOUR_APP_KEY")
              ..setAndroidAuthKey("YOUR_ANDROID_AUTH_KEY")
              ..setIOSAuthKey("YOUR_IOS_AUTH_KEY")
        $InTrack.initWithConfig(config).then((value) {
          print('$inTrack $value');
          //you can enable push notification here
        });
      }
  });
`
}
```

:::info

Check your **APP_KEY** and **AUTH_KEY**, in [inTrackPanel](https://dash.intrack.ir)->setting->SDK->Initialize, choose SDK type in **Integration Guide for** dropdown, Now your keys are in Initialization step.

:::

To configure the SDK during initialization, use a configuration object called `$InTrackConfig`. This is done by creating an instance of `$InTrackConfig` and calling its provided functions to enable the functionality you need.

:::tip[ **Congratulations** ]

You have successfully integrated inTrack with your Flutter apps and are sending system events data to your [dashboard](https://dash.intrack.ir). Please note that it may take up to a few minutes for data to reflect in your [dashboard](https://dash.intrack.ir).

:::

## Advance features
### Logging

To receive **inTrack** internal logs and debug messages about its internal state and encountered problems, you can enable logging during initialization. This can be helpful for troubleshooting and monitoring the SDK's behavior.

```dart
{
  `$InTrackConfig config = $InTrackConfig(APP_KEY)
    ..setAndroidAuthKey(ANDROID_AUTH_KEY)
      ..setIOSAuthKey(IOS_AUTH_KEY);
      config.setLoggingEnabled(true);
`
}
```

Enabling logging can provide valuable insights into the SDK's operations, aiding in diagnosing issues and ensuring smooth functionality.

### Crash Reporting
The **inTrack** SDK for Android offers robust crash reporting capabilities, allowing you to collect crash reports for later examination and resolution on the [inTrack dashboard](https://dash.intrack.ir).

**Enabling automatic crash reporting**
To activate automatic crash reporting, simply enable the feature in your initialization configuration. This will automatically catch uncaught exceptions and send them to the [inTrack dashboard](https://dash.intrack.ir) once the app relaunches and the SDK initializes.

```dart
{
  `$InTrackConfig config = $InTrackConfig(APP_KEY)
        ..setAndroidAuthKey(ANDROID_AUTH_KEY)
        ..setIOSAuthKey(IOS_AUTH_KEY)
        ..enableCrashReporting();
`
}
```

If you want to catch Dart errors, run your app inside a Zone and supply InTrack.recordDartError to the onError parameter:

```dart
{
  `void main() {
    runZonedGuarded<Future<void>>(() async {
      runApp(MyApp());
    }, $InTrack.recordDartError);
  }
`
}
```

**Logging handled exceptions**
You can also log handled exceptions during your app's runtime to monitor their occurrence and impact. Use the following command to log these exceptions:

```dart
{
  `$InTrack.logException('This is a manually created exception', false);
`
}
```

If you have handled an exception and it turns out to be fatal to your app, you can log it using the following call:

```dart
{
  `$InTrack.logException('This is a manually created exception', true);
`
}
```

You can also send the exception itself:

```dart
{
  `$InTrack.logExceptionEx(e as Exception, true);
`
}
```

By logging both automatic and handled exceptions, you can gain valuable insights into your app's stability and performance.

### Google advertising id collection

In android devices, by default, **inTrack** SDK will try to get and collect google advertisingId (if you implement it in your app), if you want to disable this functionality you shoud call seCollectAdvertisingId on config object.

```dart
{
  `$InTrack.isInitialized().then((bool isInitialized) {
    if (!isInitialized) {
      $InTrackConfig config =
          $InTrackConfig("YOUR_APP_KEY")
            ..setAndroidAuthKey("YOUR_ANDROID_AUTH_KEY")
            ..setIOSAuthKey("YOUR_IOS_AUTH_KEY")
            ..seCollectAdvertisingId(false); //<--HERE: disable google AdId collection
        $InTrack.initWithConfig(config).then((value) {
          print('$inTrack $value');
          //you can enable push notification here
        });
      }
  });
`
}
```
