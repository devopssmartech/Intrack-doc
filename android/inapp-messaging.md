---
title: In-app Messaging
version: 3.0.0+
source_path: android/inapp-messaging.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/android/inapp-messaging.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/android/inapp-messaging.md
---
# In-app Messaging

## Initialization

you can enable In App messaging service by calling `enableInAppMessaging()` on InTrackConfig object during **inTrack** initialization.

:::warning

for using In App messaging you should use **inTrack** sdk (version 2 or higher).

:::

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .enableInAppMessaging() // <-- HERE
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
        .enableInAppMessaging() // <-- HERE
)`
}
```

you can also pass 2 boolean value to the `enableInAppMessaging` method for enable/disable AutoTracking and define the way that how we store activity and fragment name by using ShortName or full class names.

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .enableInAppMessaging(true, true) // <-- HERE
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
        .enableInAppMessaging(
            enableAutoTracking: true,
            useActivityShortName: true
        ); //<-- HERE
)`
}
```

## Activity And Fragments auto tracking

by defualt **inTrack** SDK, tracking user's navigation through your app by tracking Activities and Fragments lifecycle. but if this feature dosen't fit your needs, you can disable it by calling `disableInAppMessagingActivityAndFragmentAutoTracking` method on InTrackConfig.

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .enableInAppMessaging()
        .disableInAppMessagingActivityAndFragmentAutoTracking(); // <-- HERE
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
        .disableInAppMessagingActivityAndFragmentAutoTracking() // <-- HERE
)`
}
```

## Custom Screen tracking

Screens are mobile equivalent of web pages, which can have associated properties. **inTrack** SDK allows you to tag whenever a user sees a screen in your Android activities.These tags allow you to pinpoint screens in your app where you would later render in-app messages using [inTrackdashboard](https://dash.intrack.ir). Note that screens are only usable for targeting in-app engagements.
you can manually tell **inTrack** about user navigation by calling **inTrack**'s `customNavigatedTo` method:

### Java

```java
{
`$InTrack.customNavigatedTo("YOUR_SCREEN_NAME");`
}
```

### Kotlin

```kotlin
{
`$InTrack.customNavigatedTo("YOUR_SCREEN_NAME")`
}
```

## Tracking Screen Data

Every screen can be associated with some contextual data, which can be part of the targeting rule for in-app messages. You can set the screen name and data in a single API call as shown below.

### Java

```java
{
`HashMap<String, Object> customScreenData = new HashMap<>();
customScreenData.put("category", "YOUR_PRODUCT_CATEGORY");
customScreenData.put("price", 100);
$InTrack.customNavigatedTo("YOUR_PRODUCT_NAME", customScreenData);`
}
```

### Kotlin

```kotlin
{
`val customScreenData = hashMapOf<String, Any>(
    "category" to "YOUR_PRODUCT_CATEGORY",
    "price" to 100
)
$InTrack.customNavigatedTo("YOUR_PRODUCT_NAME", customScreenData)`
}
```

marketing agents can run campaigns based on number of pages that a user navigates through you app, so each time you call `customNavigatedTo` we will increase user's page view count.
but if you want just update page without increasing user's page veiw, you can call **inTrack** `updateScreenData` method.

### Java

```java
{
`HashMap<String, Object> customScreenData = new HashMap<>();
customScreenData.put("category", "YOUR_PRODUCT_CATEGORY");
customScreenData.put("price", 100);
$InTrack.updateScreenData("YOUR_PRODUCT_NAME", customScreenData);`
}
```

### Kotlin

```kotlin
{
`val customScreenData = hashMapOf<String, Any>(
    "category" to "YOUR_PRODUCT_CATEGORY",
    "price" to 100
)
$InTrack.updateScreenData("YOUR_PRODUCT_NAME", customScreenData)`
}
```

:::note

custom screen data can be one of these data types: String, Number and Boolean.

:::

## Callbacks

Callbacks are useful for understanding the lifecycle stages of **inTrack** messages. All **inTrack** callbacks are called on the main thread.
If you want to get notified about user reactions to Messages, first you should implement **InAppMessageListener** interface:

### Java

```java
{
`class MyInAppListener implements InAppMessageListener {
    @Override
    public void onMessageTrigger(IAMMessage details) {
        Log.d("[$InTrack]", "inApp message triggered ...");
    }

    @Override
    public void onMessageClick(IAMMessage details) {
        Log.d("[$InTrack]", "inApp message clicked ...");
    }

    @Override
    public void onMessageDismiss(IAMMessage details) {
        Log.d("[$InTrack]", "inApp message dismissed ...");
    }
}`
}
```

### Kotlin

```kotlin
{
`val myInAppListener = object : InAppMessageListener {
    override fun onMessageTrigger(details: IAMMessage) {
        Log.d("[$InTrack]", "inApp message triggered ...")
    }

    override fun onMessageClick(details: IAMMessage) {
        Log.d("[$InTrack]", "inApp message clicked ...")
    }

    override fun onMessageDismiss(details: IAMMessage) {
        Log.d("[$InTrack]", "inApp message dismissed ...")
    }
}`
}
```

**on Message Trigger:** This callback is invoked before the in-app message is shown to your users.

**on Message Click:** This callback is invoked when the user clicks the message.

**on Message Dismiss:** This callback is invoked when the user dismisses the messages.

and set this class as InAppListener through InTrackConfig as shown below:

### Java

```java
{
`$InTrack.init(
    new $InTrackConfig()
        .setApplication(this)
        .setAppKey("APP_KEY")
        .setAuthKey("AUTH_KEY")
        .enableInAppMessaging()
        .setInAppListener(new MyInAppListener()); // <-- HERE
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
        .setInAppListener(MyInAppListener()) // <-- HERE
)`
}
```

:::info

You can check InApp system events [here](../events#inapp-system-events)

:::
