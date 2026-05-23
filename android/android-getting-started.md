---
title: Getting Started
version: 3.0.0+
source_path: android/android-getting-started.md
docs_url: https://docs.intrack.ir/docs/getting-start/android/android-getting-started
---
# Getting Started

## 1.Installation
**inTrack** Android SDK is hosted on mavenCentral Maven repository.
Add dependencies of **inTrack** in the `app/build.gradle` file. You can check latest version in change log.

**PreRequirement:**
* minSDKVersion: 21
* compileSdkVersion: 35



### Groovy

```Groovy
{
`dependencies {
    implementation 'ir.smartech.$intrack:android-sdk:3.0.3'
}`
}
```



### KotlinDSL

```Kotlin
{
`dependencies {
    implementation("ir.smartech.$intrack:android-sdk:3.0.3")
}`
}
```



### ProGuard Rules
If you enable minify and shrinking (`minifyEnabled true` & `shrinkResources true`),
add the following rules to your ProGuard file (proguard-rules.pro):

```
-dontwarn io.reactivex.**
-keep class io.reactivex.** { *; }
-keep class ir.intrack.android.sdk.** { *; }
-keep class sdk.main.core.** { *; }
```

## 2. Initialization
 Initialize **inTrack** SDK with your `APP_KEY` and `AUTH_KEY` from onCreate callback of your Application or MainActivity class as shown below.

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
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
)`
}
```



:::info

Check your **APP_KEY** and **AUTH_KEY**, in [inTrackPanel](https://dash.intrack.ir)->setting->SDK->Initialize, choose SDK type in **Integration Guide for** dropdown, Now your keys are in Initialization step

:::

Also, the following call should be added in `onStart` method of the main activity:



### Java

```java
{
`$InTrack.onStart(this);`
}
```

### kotlin

```kotlin
{
`$InTrack.onStart(this)`
}
```

To configure the SDK during init, a config object called <code>InTrackConfig</code> is used. Configuration is done by creating such an object and then calling it's provided function calls to enable functionality you need. Afterward that config object is provided to the "init" method.

If SDK initialized successfully you can see:  <code>Initializing[inTrack] [java-native-android] SDK version [sdk_version]</code> in your Logcat.

:::tip[**Congratulations** ]

You have now successfully integrated the **inTrack** SDK with your Android app and are sending system events data to [your dashboard](https://dash.intrack.ir). Please note that it may take up to a few minutes for your data to reflect on the [dashboard](https://dash.intrack.ir).

:::

## Advance features
### Logging
During **inTrack** initialization, you could enable logging. If logging is enabled, then **inTrack** SDK will print out debug messages about its internal state and encountered problems. Those messages may be screened in logcat and may use Androids internal log calls.

Call setLoggingEnabled on the config class to enable logging:

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .setLoggingEnabled(true) //<-- HERE
);`
}
```

### kotlin

```kotlin
{
`$InTrack.init(
    $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .setLoggingEnabled(true) //<-- HERE
)`
}
```

### Crash Reporting
The **inTrack** SDK for Android has the ability to collect crash reports, which you may examine and resolve later on the [inTrack's dashboard](https://dash.intrack.ir).

**Enabling automatic crash reporting**
To enable automatic crash reporting, call the following function on the config object. During (or after) init, this will enable crash reporting, which will automatically catch uncaught Java exceptions. They will be sent to the [dashboard](https://dash.intrack.ir) once the app is launched again and the SDK is initiated.

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .enableCrashReporting(); //<-- HERE
);`
}
```

### kotlin

```kotlin
{
`$InTrack.init(
    $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .enableCrashReporting(); //<-- HERE
)`
}
```

**Logging handled exceptions**

You might catch an exception or similar error during your app’s runtime.

You may also log these handled exceptions to monitor how and when they are happening with the following command:

### Java

```java
{
`$InTrack.recordHandledException(Exception exception);`
}
```

### kotlin

```kotlin
{
`$InTrack.recordHandledException(exception)`
}
```

If you have handled an exception, and it turns out to be fatal to your app, you may use this call:

### Java

```java
{
`$InTrack.recordUnhandledException(Exception exception);`
}
```

### kotlin

```kotlin
{
`$InTrack.recordUnhandledException(exception)`
}
```

### Google advertising id collection

In android devices, by default, **inTrack** SDK will try to get and collect google advertisingId (if you implement it in your app), if you want to disable this functionality you shoud call seCollectAdvertisingId on config object.

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .seCollectAdvertisingId(false) // <-- HERE
);`
}
```

### kotlin

```kotlin
{
`$InTrack.init(
    $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .seCollectAdvertisingId(false) // <-- HERE
)`
}
```
