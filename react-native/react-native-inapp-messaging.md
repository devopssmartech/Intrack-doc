---
title: In-app Messaging
version: 3.0.0+
source_path: react-native/react-native-inapp-messaging.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/react-native/react-native-inapp-messaging.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/react-native/react-native-inapp-messaging.md
---
# In-app Messaging

## Initialization

you can enable In App messaging service by calling **enableInAppMessaging** on config object during **inTrack** initialization.

:::warning

for using In App messaging you should use inTrack sdk (version 2 or higher)

:::

```javascript
{
  `if (!(await $InTrack.isInitialized())) {
    const options = {
      appKey: 'APP_KEY',
      iosAuthKey: 'ّAUTH_KEY_FOR_IOS',
      androidAuthKey: 'AUTH_KEY_FOR_ANDROID',
      enableInAppMessaging: true // <-- here
    };
    await $InTrack.init(options);
    $InTrack.start();
  }
`
}
```

:::note

At present, In App Messaging functionality is exclusively available for Android devices.

:::

## Custom Screen tracking

Screens are mobile equivalent of web pages, which can have associated properties. **inTrack** SDK allows you to tag whenever a user sees a screen.These tags allow you to pinpoint screens in your app where you would later render in-app messages using [inTrack dashboard](https://dash.intrack.ir). Note that screens are only usable for targeting in-app engagements.
you can manually tell **inTrack** about user navigation by calling inTrack's recordNavigation method:

```javascript
{
  `$InTrack.recordNavigation('YOU_SCREEN_NAME');
`
}
```

## Tracking Screen Data

Every screen can be associated with some contextual data, which can be part of the targeting rule for in-app messages. You can set the screen name and data as shown below:

```javascript
{
  `$InTrack.recordNavigation('YOU_SCREEN_NAME', {
      //your screen data such as :
      category: 'YOUR_CATEGORY',
      price: 100,
    });
`
}
```

marketing agents can run campaigns based on number of pages that a user navigates through you app, so each time you call recordNavigation we will increase user's page view count.

## Automatic Screen tracking

Standard React Native applications run inside a single Activity/ViewController, meaning any screen changes won't be tracked by the native **inTrack** SDKs. There are a number of ways to implement navigation within React Native apps, therefore there is no "one fits all" solution to screen tracking, however in the following section we review some technique for popular library.

### React Navigation

The NavigationContainer component which the library exposes provides access to the current navigation state when a screen changes, allowing you to use the `recordNavigation` method for automatic screen tracking. For more information please see the [official document of React Navigation](https://reactnavigation.org/docs/screen-tracking/).

```javascript
{
  `import {
    NavigationContainer,
    useNavigationContainerRef,
    } from '@react-navigation/native';

    export default () => {
      const navigationRef = useNavigationContainerRef();
      const routeNameRef = useRef();
      return (
        <NavigationContainer
        ref={navigationRef}
        onReady={() => {
          routeNameRef.current = navigationRef.getCurrentRoute().name;
          }
        onStateChange={async () => {
          const previousRouteName = routeNameRef.current;
          const currentRouteName = navigationRef.getCurrentRoute().name;
          if (previousRouteName !== currentRouteName) {
              //Save the current route name for later comparison
              routeNameRef.current = currentRouteName;

              // send currentRouteName to  $InTrack
              $InTrack.recordNavigation(currentRouteName,{});
              }
              }}
                >
                {/* ... */}
                </NavigationContainer>
                );
      };
`
}
```

### React Native Navigation

To manually track screens, you need to setup a componentDidAppear event listener and manually call the recordNavigation method, To learn more, view the [events documentation](https://wix.github.io/react-native-navigation/api/events#componentdidappear) on the React Native Navigation website.

```javascript
{
  `import {Navigation} from 'react-native-navigation';
    Navigation.events().registerComponentDidAppearListener(({ componentName, componentType }) => {
      if (componentType === 'Component') {
        $InTrack.recordNavigation(componentName,{});
        }
    });
`
}
```

## Callbacks

To receive notifications about user interactions with In App Messages in your React Native application, you can register callback functions as a below:


```javascript
{
  `$InTrack.onInAppMessageTriggered(message => {
      console.log('inApp message triggered', message);
    });

  $InTrack.onInAppMessageClicked(message => {
      console.log('inApp message clicked', message);
    });

  $InTrack.onInAppMessageDismissed(message => {
      console.log('inApp message dissmissed', message);
    });
`
}
```

**on Message Trigger:** This callback is invoked before the in-app message is shown to your users.

**on Message Click:** This callback is invoked when the user clicks the message.

**on Message Dismiss:** This callback is invoked when the user dismisses the messages.

:::info

You can checked InApp system events [here](../events#inapp-system-events)

:::
