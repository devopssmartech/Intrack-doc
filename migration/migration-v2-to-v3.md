---
title: Migrating from SDK V2 to V3
version: 3.0.0+
source_path: migration/migration-v2-to-v3.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/migration/migration-v2-to-v3.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/migration/migration-v2-to-v3.md
---
# Migrating from SDK V2 to V3

This guide provides step-by-step instructions to help you upgrade your integration from **Version 2** to **Version 3** of the SDK. It outlines breaking changes, updated APIs, and best practices to ensure a smooth migration.

## Android
### In-App Messaging Theme Configuration
Starting from SDK v3.0, you no longer need to set the in-app message theme in your code.
All styling—colors, fonts, and layout—is now managed directly from the dashboard panel and automatically applied.

✅ If you're upgrading to SDK v3.0 or above, make sure to remove any `setInAppMessagesDefaultTheme()` calls from your code.

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .enableInAppMessaging()
        .setInAppMessagesDefaultTheme(  //<========= Remove this method
            new $InTrackInAppMessageTheme(
                "#000000", // titleColor
                "#000000", // descriptionColor
                "#FFFFFF", // backgroundColor
                "#000000", // actionLabelColor
                "#FFFFFF", // actionBackgroundColor
                FontFamily.B_YEKAN // customFont
            )
        )
);`
}
```

### Kotlin

```kotlin
{
`$InTrack.init(
    $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .enableInAppMessaging()
        .setInAppMessagesDefaultTheme( //<========= Remove this method
            $InTrackInAppMessageTheme(
                titleColor = "#000000",
                descriptionColor = "#000000",
                backgroundColor = "#FFFFFF",
                actionLabelColor = "#000000",
                actionBackgroundColor = "#FFFFFF",
                FontFamily.B_YEKAN
            )
        )
)`
}
```

## Web
### On-Site Theme Configuration
Starting from SDK v3.0, all theme settings like background color, text color, border radius, and CTA styles—are managed directly from the dashboard.

Your onsite campaigns will automatically use the styles set in the panel and no need to define them in code anymore.
You can remove the `baseTheme` config from your code when calling the method [`initOnSiteMessaging`](../web/on-site.md#initialization).

```javascript
{
`config: {
    baseTheme: { //<========= Remove this
        bg_color: '#fefefe',
        border_radius: '8',
        label_color: '#000000',
        title_color: '#1B1B1D',
        description_color: '#38373B',
        cta_font_color: '#fff',
        cta_background: '#1D88F6',
        cta_background_coupon: '#FAFAFA',
        cta_font_color_coupon: '#00587A'
    },
    displayMessage: null,
    disableAutoTracking: false
}`
}
```

## iOS
### In-App Messaging Theme Configuration

Starting from SDK v3.0 the SDK has been renamed from **InTrack** to **InTrackSDK**,
If you're upgrading from an earlier version (e.g., v2.x), make sure to update your Podfile:

```ruby
{
  `pod '$InTrackSDK'`
}
```

All theme-related settings have been removed from the SDK side.
you can remove `setInAppTheme` config from your code.

### Objective-C

```objectivec
{
  `$InTrackConfig* config = $InTrackConfig.new;
  config.appKey = @"YOUR_APP_KEY";
  config.authKey = @"YOUR_AUTH_KEY";
  config.enableInAppMessaging = YES;
  config.inAppDisplayDelegate = self;
  [config setInAppTheme:@"#000000"
      backgroundColor:@"#00a1b1"
            bodyColor:@"#000000"
      actionLabelColor:@"#860202"
  actionBackgroundColor:@"#FFFFFF"];  //<========= Remove this
  [$InTrack startWithConfig:config];
`
}
```

### swift

```swift
{
  `let config: $InTrackConfig = $InTrackConfig()
  config.appKey = @"YOUR_APP_KEY"
  config.authKey = @"YOUR_AUTH_KEY"
  config.enableInAppMessaging = true
  config.inAppDisplayDelegate = self
  config.setInAppTheme("#000000", backgroundColor: "#00a1b1", bodyColor: "#000000", actionLableColor: "#860202", actionBackgroundColor: "#FFFFFF") //<========= Remove this
  $InTrack.initWith(config)
`
}
```

## Flutter
### In-App Messaging Initialization
From v3, for Android platform, you should make sure that your Android's `MainActivity` extends `FlutterFragmentActivity` or any other activity that have set
[`ViewTreeLifeCycleOwner`](https://developer.android.com/reference/androidx/lifecycle/ViewTreeLifecycleOwner) on its [`decorView`](https://developer.android.com/reference/android/view/Window#getDecorView())

If you are extending from `FlutterActivity` you can change it to `FlutterFragmentActivity`. the final code should be like the code below:

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

### In-App Messaging Theme Configuration
Starting from SDK v3.0, all theme-related settings have been removed from the SDK side.
You can remove the `defaultTheme` config from your code when calling the method [`enableInAppMessaging`](../flutter/inapp-messaging.md#message-theme)

```dart
{
  `$InTrack.isInitialized().then((bool isInitialized) {
    if (!isInitialized) {
      $InTrackConfig config =
      $InTrackConfig("YOUR_APP_KEY")
        ..setAndroidAuthKey("YOUR_ANDROID_AUTH_KEY")
        ..setIOSAuthKey("YOUR_IOS_AUTH_KEY")
          ..enableInAppMessaging(
            defaultTheme: {  //<========= Remove this
              'titleColor': '#fc0330',
              'descriptionColor': '#000000',
              'bgColor': '#ffffff',
              'actionLabelColor': '#000000',
              'actionBgColor': '#ffffff'
          });
    $InTrack.initWithConfig(config).then((value) {
      print('$inTrack $value');
      //you can enable push notification here
    });
  }
});
`
}
```

## React Native
### In-App Messaging Theme Configuration

Starting from SDK v3.0, all theme-related settings have been removed from the SDK side.
you can remove `inAppDefaultTheme` config from your code.

```javascript
{
`if (!(await $InTrack.isInitialized())) {
    const options = {
      appKey: 'APP_KEY',
      iosAuthKey: 'ّAUTH_KEY_FOR_IOS',
      androidAuthKey: 'AUTH_KEY_FOR_ANDROID',
      enableInAppMessaging: true,
      inAppDefaultTheme: { //<========= Remove this
        titleColor: '#000000',
        descriptionColor: '#000000',
        bgColor: '#ffffff',
        actionLabelColor: '#000000',
        actionBgColor: '#ffffff'
      }
    };
    await $InTrack.init(options);
}
$InTrack.start();`
}
```
