---
title: Getting Started
version: 3.0.0+
source_path: react-native/react-native-getting-started.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/react-native/react-native-getting-started.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/react-native/react-native-getting-started.md
---
# Getting Started

Here's how you can integrate the **inTrack** SDK with your react native apps:

## 1. Install React Native Library

Use the below command in your project directory to install **inTrack** React Native library.

### npm

```npm
{
`npm install --save $intrack-react-native-bridge@3.0.3`
}
```

### Yarn

```yarn
{
`yarn add $intrack-react-native-bridge@3.0.3`
}
```



:::warning

Don't forget to link the library with your project in case you are using a React Native version lower than 0.60 as a below:

:::

```yarn
{
`react-native link $intrack-react-native-bridge`
}
```

for iOS we are using CocoaPods for auto-linking, so you need to install Pods

```
(cd ios && pod install) # CocoaPods on iOS needs this extra step
```

### Proguard Rule

In case of using proguard, the following rule should be added to proguard settings:

```
-keep class com.google.android.gms.ads.**{*;}
-dontwarn io.reactivex.**
-keep class io.reactivex.** { *; }
-keep class ir.intrack.android.sdk.** { *; }
-keep class sdk.main.core.** { *; }
```

## 2. Initialization

Grab a reference to the inTrack React Native library in your JavaScript file.

### Java

```javascript
{
`import $InTrack from '$intrack-react-native-bridge';`
}
```



In order to initialize the **inTrack** SDK, two init and start methods should be called. These methods should be called once in application's lifecycle, and it should be done as early as possible. Main app component's componentDidMount method is a good place.

### Java

```javascript
{
  `if (!(await $InTrack.isInitialized())) {
    const options = {
        appKey: 'APP_KEY',
        iosAuthKey: 'ّAUTH_KEY_FOR_IOS',
        androidAuthKey: 'AUTH_KEY_FOR_ANDROID',
    };
    await $InTrack.init(options);
    $InTrack.start();
  }
`
}
```



:::info

Check your **APP_KEY** and **AUTH_KEY**, in [inTrackPanel](https://dash.intrack.ir)->setting->SDK->Initialize, choose SDK type in **Integration Guide for** dropdown, Now your keys are in Initialization step.

:::

:::warning

If version 1.2.1 or older used, please init as a below:

:::

```javascript
{
  `if (!(await $InTrack.isInitialized())) {
    const appKey = 'APP_KEY';
    const authKey =
    Platform.OS === 'ios' ? 'ّAUTH_KEY_FOR_IOS' : 'AUTH_KEY_FOR_ANDROID';
    await $InTrack.init(appKey, authKey);
      $InTrack.start();
  }
`
}
```

:::tip[ **Congratulations** ]

You have successfully integrated **inTrack** with your React Native apps and are sending system events data to your [dashboard](https://dash.intrack.ir). Please note that it may take up to a few minutes for data to reflect in your [dashboard](https://dash.intrack.ir).

:::

## Advance features
### Logging
To receive **inTrack** internal logs and debug messages about its internal state and encountered problems, you can enable logging during initialization. This can be helpful for troubleshooting and monitoring the SDK's behavior.

```javascript
{
  `if (!(await $InTrack.isInitialized())) {
  const options = {
    appKey: 'APP_KEY',
    iosAuthKey: 'ّAUTH_KEY_FOR_IOS',
    androidAuthKey: 'AUTH_KEY_FOR_ANDROID',
    loggingEnabled: true, // <-- HERE
  };
  await $InTrack.init(options);
  $InTrack.start();
  }
`
}
```

Enabling logging can provide valuable insights into the SDK's operations, aiding in diagnosing issues and ensuring smooth functionality.

### Crash Reporting
The **inTrack** SDK for Android offers robust crash reporting capabilities, allowing you to collect crash reports for later examination and resolution on the [**inTrack** dashboard](https://dash.intrack.ir).

**Enabling automatic crash reporting**
To activate automatic crash reporting, simply enable the feature in your initialization configuration. This will automatically catch uncaught Java exceptions and send them to the [dashboard](https://dash.intrack.ir) once the app relaunches and the SDK initializes.

```javascript
{
  `const options = {
    appKey: 'APP_KEY',
    iosAuthKey: 'ّAUTH_KEY_FOR_IOS',
    androidAuthKey: 'AUTH_KEY_FOR_ANDROID',
    enableCrashReporting: true,
  };
  await $InTrack.init(options);
  $InTrack.start();
`
}
```

**Logging handled exceptions**
You can also log handled exceptions during your app's runtime to monitor their occurrence and impact. Use the following command to log these exceptions:

```javascript
{
  `$InTrack.logException('SOME ERROR HAPPEND!', false);
`
}
```

If you encounter a handled exception that proves to be fatal to your app, use this call instead:

```javascript
{
  `$InTrack.logException(error, true);
`
}
```

By logging both automatic and handled exceptions, you can gain valuable insights into your app's stability and performance.

### Setting Geographical Position

Before initializing the **inTrack** SDK, you have the option to set the geographical position of the device using the setLatitude and setLongitude methods. This allows you to provide accurate location data for enhanced targeting and personalized experiences.

### Java

```javascript
{
  `const options = {
    appKey: 'APP_KEY',
    iosAuthKey: 'ّAUTH_KEY_FOR_IOS',
    androidAuthKey: 'AUTH_KEY_FOR_ANDROID',
    setLatitude: 'Latitude',
    setLongitude: 'Longitude'
  };
  await $InTrack.init(options);
  $InTrack.start();
`
}
```
