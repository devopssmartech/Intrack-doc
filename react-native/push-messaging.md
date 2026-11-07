---
title: Push Messaging
version: 3.0.0+
source_path: react-native/push-messaging.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/react-native/push-messaging.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/react-native/push-messaging.md
---
# Push Messaging

:::warning

  Before continuing, please ensure that you have added [react-native-inTrack](react-native-getting-started.md) to your App.

:::

## Android
You can incorporate push messaging into your React Native application with react-native-inTrack through two different methods.

* React Native Approach
* Native Approach

:::info

  If you're using multiple Firebase messaging providers, we suggest following Approach 2.

:::

### Approach 1: React Native Approach (Using Firebase Plugin)

#### Step 1: Integrating firebase/messaging with your React-Native Android App

:::info

  If you've already integrated @react-native-firebase/messaging, you can skip Step 1 and proceed directly to Step 2.

:::

For optimal integration, we recommend utilising the @react-native-firebase/messaging library exclusively for Android platform. Implement methods with help of [React Native Firebase Messaging Docs](https://rnfirebase.io/messaging/usage) to facilitate seamless inTrack integration with push notifications.

#### Step 2: Pass Push Token To inTrack

In the App.js file, inside the useEffect block, obtain the FCM token using messaging().getToken() and pass it to inTrack like this:

```javascript
{
  `useEffect(() => {
    await messaging().registerDeviceForRemoteMessages();

    // Get Token From Firebase
    const token = await messaging().getToken();

    // Pass Token to $InTrack
    $InTrack.onTokenRefresh(token);
  }, []);
`
}
```

#### Step 3: Integrating inTrack with your React-Native Android App

Assuming you have integrated firebase using @react-native-firebase/messaging and implemented all the necessary methods outlined in the [React Native Firebase Messaging Docs](https://rnfirebase.io/messaging/usage) for Android, Initializing inTrack notification service is done by calling InTrack.initNotificationService. This method should be called before start and after init:

```javascript
{
  `const initIntrack = async () => {
      if (!(await $InTrack.isInitialized())) {
        const options = {
        appKey: 'APP_KEY',
        iosAuthKey: 'ّAUTH_KEY_FOR_IOS',
        androidAuthKey: 'AUTH_KEY_FOR_ANDROID',
        loggingEnabled: true,
      };
      await $InTrack.init(options);
      $InTrack.initNotificationService(); <-- Here
      $InTrack.start();
      }
      };
`
}
```

After calling this method, notification channels may be created and registering to a push service may be done.

#### Step 4: Create the notification channel

Android requires channels for notifications starting from Android 8.0 (API level 26). You can create a channel in your React Native project using the react-native-push-notification library or the Firebase messaging API directly.

```javascript
{
  `import PushNotification from 'react-native-push-notification';
    PushNotification.createChannel(
    {
      channelId: "default-channel-id", // (required)
      channelName: "Default Channel", // (required)
      importance: 4, // (optional) Make sure importance set to  High importance to displaying push notification when your app is in a background
    },

    (created) => console.log("createChannel returned \${created}") // (optional) callback returns whether the channel was created, false means it already existed.
    );
`
}
```

#### Step 5: Pass push payload to inTrack

Whenever a push message is received, the inTrack SDK should be notified by calling the inTrack.pushMessageReceived(message, channelId) method. Here, channelId specifies the channel on which the notification should be displayed, This method should be called from the following Firebase methods

* `onMessage`: Invoke the inTrack's pushMessageReceived method within this method to render foreground notifications.

```javascript
{
  `messaging().onMessage(async remoteMessage => {
    $InTrack.pushMessageReceived(remoteMessage.data, channelId) <--- Here
  });
`
}
```

* `setBackgroundMessageHandler`: Invoke the inTrack's onMessageReceived method within this handler method to render background notifications.

```javascript
{
  `messaging().setBackgroundMessageHandler(message => {
      $InTrack.pushMessageReceived(message.data, channelId) <--- Here
      return Promise.resolve();
    });
`
}
```

* channelId: The identifier for the notification channel.
* message: The message object containing the following model:

```json
  message: {
    id: String,
    title: String,
    message: String,
    sound: String,
    link: String,
    media: String,
    buttons: String,
  }
```

These fields are sent by inTrack server in data fields of push message.

:::note

To displaying push notification when your app is in a background, you need to register android push notification channel with High importance. you need to send the ID of that channel to the inTrack.pushMessageReceived method (channelId). To setup a background handler, call the setBackgroundMessageHandler outside of your application logic as early as possible.

:::

:::note

if you want to make sure that message is sent by inTrack, you should check source key of message data. note that inTrack always send data push message and source key is equal to inTrack

:::

### Approach 2: Native Approach

Refer to the [Android Push Messaging Guide](../android/push-messaging/push-messaging.md) for instructions on setting up Push Messaging for your React Native Android application.

### Making your app compatible with Android 13 push changes

From Android 13 onwards, clients will have to explicitly ask permissions from end user to send push notifications. This means, client will NOT receive push opted in as true once the app is installed by the end user, unless the user explicitly subscribes for same.
To make sure your app is compatible with Android 13 changes, Kindly refer to [official Google documentation](https://developer.android.com/guide/topics/ui/notifiers/notification-permission) to make your application compatible with the same.

### Timer Count Down push

for this section please refer to the section [Timer Count Down Push Messaging](../android/push-messaging/push-messaging.md#timer-count-down-push) in android push notification setup.

### Push Notification icon

in android you chould change push notification icon by adding the folowing meta tag to the [AndroidManifest](https://github.com/smartech-ir/intrack-react-native-sdk-demo/blob/master/android/app/src/main/AndroidManifest.xml)

## iOS

For using inTrack Push Notifications on iOS apps you need to enable Push Notifications and the Remote notifications Background Mode for your target (under the Capabilities section of Xcode).

after initialze inTrack, you'll need to initalize push notifications by calling InTrack.initNotificationService method, The inTrack iOS SDK will automatically handle the rest. No need to call any other method for registering when a device token is generated, or a push notification is received.

please note that you also need to ask for user's permission for push notifications, if you didn't already get the permission, you can pass the related option to initNotificationService method.

```javascript
{
  `$InTrack.initNotificationService({
     askForUserPermission: true, // the default value is false, you can set it to true to get user permissions here.
  });
`
}
```

#### Rich Push Notifications
For enabling Rich Push Notification on iOS follow the [related instuction](../react-native/push-messaging.md) on our ios docs.

## Callback

if you want to get notified when user clicked on an push notification that sent by inTrack, you can register a callBack function with `onNotificationClicked` method:

```javascript
{
  ` $InTrack.onNotificationClicked(event => {
      console.log('push notification clicked:', event);
    });
`
}
```

:::info

You can checked push notification system events [here](../events#push-system-events)

:::
