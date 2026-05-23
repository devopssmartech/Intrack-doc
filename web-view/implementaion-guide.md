---
title: Integration Guide
version: 3.0.0+
source_path: web-view/implementaion-guide.md
docs_url: https://docs.intrack.ir/docs/getting-start/web-view/implementaion-guide
---
# Integration Guide

If your application is designed to display website pages within the app (WebView), you can use the automatic sync feature of the Web SDK to seamlessly synchronize devices in  **inTrack**.

In this setup, you send a deviceId to the webpage, notifying the **inTrack** Web SDK that the page is loaded within a WebView linked to a device already registered in **inTrack**. As a result, events sent from the web are automatically synchronized with the events sent from the native app, ensuring seamless integration.

## Implementation Process

The implementation of this feature involves the following four steps, which will be explained in detail:

1. Adding the relevant authKeys for Android and iOS to the configuration of the **inTrack** Web SDK.
2. Specifying the execution environment for the **inTrack** Web SDK.
3. Retrieving the deviceId from the native **inTrack** SDKs.
4. Sending the deviceId to the WebView.

### 1. Adding AuthKeys for Other Platforms to the Web SDK

Typically, the configuration of the **inTrack** SDKs requires two parameters: **AppKey** and **AuthKey**. These values can be found in the [**inTrack** dashboard](https://dash.intrack.ir) under the Settings section on the SDK page.

At this stage, simply copy the `auth_key` for both Android and iOS from the  [dashboard](https://dash.intrack.ir) and add them to the configuration of the Web SDK.

```javascript
{
`var $inTrack_config = {
    app_key: 'Provided_App_Key',
    auth_key: 'Provided_Auth_Key_for_Web',
    public_key: 'Provided_Public_Key',
    android_auth_key: 'Provided_Auth_key_for_Android', // HERE
    ios_auth_key: 'Provided_Auth_key_for_iOS' // HERE
};`
 }
```

### 2. Specifying the Execution Environment for the Web SDK

At this stage, it is necessary to inform the **inTrack** Web SDK that it is running within a WebView and should not start functioning until the `deviceId` is received.

This can be done in two ways:

#### 2.1. From Native Code:

If you want to inform the **inTrack** SDK from the native code that it is running within a WebView, you need to pass the query string **intrack_device_id=-1** to the webpage. For example, use a URL like this:

```javascript
{
`https://yourdomain.com?$intrack_device_id=-1`
 }
```

**Kotlin Code for Passing `deviceId` to WebView**

```kotlin
{
`val webView = view.findViewById<WebView>(R.id.webview)  // Find the WebView
val webSettings = webView.settings  // Get WebView settings

// Enable JavaScript and DOM storage for the WebView
webSettings.javaScriptEnabled = true
webSettings.setDomStorageEnabled(true)

// Load the URL with the query parameter to notify Web SDK
webView.loadUrl("https://yourdomain.com?$intrack_device_id=-1")`
}
```

#### 2.2. From the Web Side

Another way to specify the WebView status is by setting the webview parameter in the configuration for the Web SDK. You can specify the environment like this:

```javascript
{
`var $inTrack_config = {
    app_key: 'Provided_App_Key',
    auth_key: 'Provided_Auth_Key_for_Web',
    public_key: 'Provided_Public_Key',
    android_auth_key: 'Provided_Auth_key_for_Android',
    ios_auth_key: 'Provided_Auth_key_for_iOS',
    webview: true,
};`
}
```

:::note
 If your web page is shared between a website and a WebView, make sure to set the webview value to true or false accordingly based on the environment.
:::

### 3. Retrieving the `deviceId` from the Native SDK

In the app, you can retrieve the deviceId assigned to the user's device by **inTrack** using the `getDeviceId()` method provided by the Native SDK.
* [Android](../android/tracking-users#getting-deviceanonymous-id)
* [iOS](../iOS/tracking-users#getting-deviceanonymous-id)

### 4. Sending `deviceId` to the WebView

Finally, you need to send the `deviceId` obtained from step 3, along with the packageName and version of your app, to **inTrack** using the injectDevice method via WebView.

Here’s the Kotlin code for injecting the deviceId into the WebView:

```kotlin
{
`webView.loadUrl("javascript:$Intk('injectDevice','{deviceId: \"$InTrack.getDeviceId()\", appId: \"context?.packageName\", appVersion: \"context.packageManager.getPackageInfo(context.packageName, 0).versionName\"}')")`
}
```

In other words, the following JavaScript code should be executed in the WebView:

```javascript
{
`$Intk("injectDevice", {deviceId: "YOUR_$INTRACK_DEVICEID", appId: "your.app.id", appVersion: "1.0.0"})`
}
```

This will pass the `deviceId`, `packageName`, and `appVersion` to the **inTrack** Web SDK.
