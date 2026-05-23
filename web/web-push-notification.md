---
title: Web Push Messaging
version: 3.0.0+
source_path: web/web-push-notification.md
docs_url: https://docs.intrack.ir/docs/getting-start/web/web-push-notification
---
# Web Push Messaging

:::warning

Please ensure that you have completed all the platform integration steps listed under [GettingStarted](web-getting-started) before proceeding.

:::

## Initialization

if you are already handling web push notification on your site and want to integrate with us, please follow the instruction on [Integration with other SDKs](#integrate-with-other-sdk).

first, you need to register our service worker, which is responsible to display web push message to your customers. for this reason please download the following file to your system and then upload them to the root of the HTTPS domain (public_html or var/www/html or a similar folder). These need to be accessible publicly from root of your domain
 `https://your-domain.com/sw.js`

```jsx
{
`https://static1.intrack.ir/api/web/download/sdk/v1/sw.js`
}
```

if you already have a service worker you can import our script in your file :

```jsx
{
`if('function' === typeof importScripts) {
    importScripts("https://static1.$intrack.ir/api/web/download/sdk/v1/inTrack-sw.min.js")
}`
}
```

please note that in order to access [PushManager](https://developer.mozilla.org/en-US/docs/Web/API/PushManager) and getting subscription status of users, our SDK need to be aware of service worker registration status, so by default we assume the script is located at root of your domain (ie: `https://your-domain.com/sw.js`), if it is not the case, you need to give your service worker path throuhg `sw_path` config in initialization of SDK.

```jsx
{
  `var $intrack_config = {
    app_key: 'Provided_App_Key',
    auth_key: 'Provided_Auth_Key',
    public_key: 'Provided_Public_Key',
    sw_path: "/path-to-your/serviceWorker.js" //<-- HERE
  }
`
}
```

Initializing **inTrack** notification service is done by calling `$InTrack.InitWebPush()` with optional config parameter. This method should be called after importing the
`$inTrack.js` file and calling `$InTrack.init()`, so the initialization block of SDK will look like this:

### Asynchronous

```jsx
{
`$Intk('InitWebPush', webPushConfig);`
}
```

### Synchronous

```jsx
{
`$InTrack.InitWebPush(webPushConfig);`
}
```

:::note

You should call this method after SDK initialization with appropriate `public_key`.

:::

Method `InitWebPush` accepts a config parameter with the following model. You can customize look and feel of widgets (welcome message and subscription bell) with these parameters:

```jsx
{
`webPushConfig: {
    subscriptionStyle: {
        subscriptionTheme: 1,
        subscriptionThemePos: 1,
        subscriptionOverlayOpacity: 0,
        subscriptionBoxColor: "#fff",
        subscriptionBtnAllowTxt: "بله",
        subscriptionBtnAllowColor: "#0e82e5",
        subscriptionBtnAllowTxtColor: "#fff",
        subscriptionBtnDenyTxt: "بعدا",
        subscriptionBtnDenyColor: "#d3d3d3",
        subscriptionBtnDenyTxtColor: "#888",
        subscriptionTitle: "!آخرین اطلاعات را از ما دریافت کنید",
        subscriptionTitleTxtColor: "#333",
        subscriptionMessage: "آیا اجازه ارسال اعلان (نوتیفیکیشن) می‌دهید؟",
        subscriptionMessageTxtColor: "#777",
        subscriptionBoxDelay: 3000
    },
    subscriptionStyleMobileSeparate: false,
    subscriptionStyleMobile: {
        subscriptionTheme: 1,
        subscriptionThemePos: 1,
        subscriptionOverlayOpacity: 0,
        subscriptionBoxColor: "#fff",
        subscriptionBtnAllowTxt: "بله",
        subscriptionBtnAllowColor: "#0e82e5",
        subscriptionBtnAllowTxtColor: "#fff",
        subscriptionBtnDenyTxt: "بعدا",
        subscriptionBtnDenyColor: "#d3d3d3",
        subscriptionBtnDenyTxtColor: "#888",
        subscriptionTitle: "!آخرین اطلاعات را از ما دریافت کنید",
        subscriptionTitleTxtColor: "#333",
        subscriptionMessage: "آیا اجازه ارسال اعلان (نوتیفیکیشن) می‌دهید؟",
        subscriptionMessageTxtColor: "#777",
        subscriptionBoxDelay: 3000
    },
    isRTL: true,
    askAgainAfterDays: 0,
    widget: {
        enable: false,
        enableUnsubs: false,
        color: '#337ab7',
        icon: "https://static1.intrack.ir/api/web/download/sdk/widget_default.png",
        style: {
            position: "left",
            tickerRight:10,
            tickerLeft: 10,
            tickerBottom: 10,
            notifLeft: 50,
            notifRight: 50,
            notifBottom: 70,
        },
        messages: {
            "sideTitle": "اعلان (نوتیفیکیشن)",
            "title":"اعلان های وب سایت",
            "body":"آخرین اطلاعات وب سایت را در لحظه دریافت کنید.",
            "moreOptions":"گزینه های بیشتر",
            "subscribe":"اشتراک",
            "unsubscribe":"لغواشتراک",
            "notNow":"الان نه",
            "cancel":"بستن",
            "unblockMobile":"شما اعلان ها را بلاک کرده اید. لطفا روی آیکون قفل در نوار آدرس کلیک کرده به بخش Site Settings رفته و در ذیل Permission گزینه  Notifications را فعال کرده و صفحه را رفرش کنید",
            "unblockChrome":"شما اعلان ها را غیرفعال کرده اید. لطفا روی آیکون کنار نوار آدرس کلیک کرده و Notification را فعال کنید",
            "unblockFirefox":"شما اعلان ها را بلاک کرده اید، لطفا روی آیکون تنظیمات در کنار نوارآدرس کلیک کرده و گزینه Blocked/Blocked Temporarily را حذف کنید",
        }
    }
}`
}
```

### webPushConfig

| Properties                      | Type    | Description                                                                                                                                                                                                                                                                     |
|---------------------------------|---------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| subscriptionTheme               | number  | There are 4 different styles that you can choose from: 0, 1, 2, 3, 4. 0 is 1-step opt-in initiated natively by the browser, requiring 1 step for the user to opt-in. The rest are 2-step opt-in initiated by inTrack, requiring 2 steps for the user to opt-in. |
| subscriptionThemePos            | number  | You can specify the position of push notification.  1: top 2: bottom  3: center                                                                                                                                                                                                 |
| subscriptionStyleMobileSeparate | boolean | Enable this feature to separate themes on mobile devices, allowing you to specify the properties of the subscriptionStyleMobile object.                                                                                                                                         |
| askAgainAfterDays               | number  | If you choose to display a welcome subscription message to the user, and the user decides to subscribe later, after this period (in days), the subscription message will be shown again. Zero means never ask again.                                                            |

**Available position for subscriptionTheme:** 1 : 1, 2, 3

**Available position for subscriptionTheme:** 2 : 1, 2

### Subscription Widget

The subscription widget resides in the lower left or right corner of your site, which users can click to bring up the Native Permission Prompt for your site. It is designed to be small enough that you may keep it on your site at all times, and does not require users to dismiss it.

![Docusaurus](/img/websdk/subwidget1.png)

you can enable subscription widget by enabling it through push config:

```jsx
webPushConfig: {
  ...
  widget: {
    enable: true,
  }
}
```

you can also customize the look of widget with following properties:

| Properties   | Type    | Description                                                                                                                                                              |
|--------------|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| enable       | Boolean | Enable or disable the widget.                                                                                                                                            |
| enableUnsubs | Boolean | Allow users to unsubscribe from web push notifications.                                                                                                                  |
| color        | String  | Background color of the widget.                                                                                                                                          |
| icon         | String  | Icon of the widget (e.g., bell).                                                                                                                                         |
| style        | Object  | If the position is set to "left", only "tickerLeft" and "notifLeft" will be considered. Similarly, "tickerRight" and "notifRight" will be used for the "right" position. |
| message      | Object  | Customize or localize the texts.                                                                                                                                         |

!![Docusaurus](/img/websdk/subwidget2.png)

![Docusaurus](/img/websdk/subwidget3.png)

![Docusaurus](/img/websdk/subwidget4.png)

### Subscription Info

You can get subscription status of user with calling `$InTrack.SubscriptionWebPushInfo()`.

### Asynchronous

```jsx
{
`$Intk('SubscriptionWebPushInfo', callBack);`
}
```

### Synchronous

```jsx
{
`$InTrack.SubscriptionWebPushInfo(callBack);`
}
```

The result specified below:

| Status       | Description                                                   |
|--------------|---------------------------------------------------------------|
| subscribed   | If the user has given permission to receive notifications.    |
| unsubscribed | If the user has unsubscribed from receiving notifications.    |
| denied       | User has declined notification permission/blocked permission. |
| canceled     | User has selected Later option on the 2-step opt-in.          |
| no_init      | PushAlert script initialization is disabled.                  |

in Asynchronous mode, it will be passed to callBack function.

### Unsubscribe

You can unsubscribe users from receiving web push notifications.

### Asynchronous

```javascript
{
`$Intk('UnsubscribeWebPush');`
}
```

### Synchronous

```javascript
{
`$InTrack.UnsubscribeWebPush();`
}
```

For Example:

### Asynchronous

```jsx
{
`<button onclick="$Intk('UnsubscribeWebPush')">Unsubscribe</button>`
}
```

### Synchronous

```jsx
{
`<button onclick="$InTrack.UnsubscribeWebPush()">Unsubscribe</button>`
}
```

### Integrate with other SDK

If you are using another SDK like [firebase-js-sdk](https://github.com/firebase/firebase-js-sdk) for handling push notification, and want to use our platform for sending web push, before initiating our SKDs please contact our support team.

based on your current implementation, we have two method for integrating with other SDKs (that described in following section)

#### Integrate with Firebase JS SDK

if you are using Firebase SDK, you don't need to call our InitWebPush method, instead you should implement the following steps:

**1- send registration token**

you need to get registration token from Firebase sdk and send it to inTrack by calling our `sendFireBaseToken`  method:

### Asynchronous

```jsx
{
`$Intk('sendFireBaseToken', token);`
}
```

### Synchronous

```jsx
{
`$InTrack.sendFireBaseToken(token);`
}
```

**Example:**

### Asynchronous

```javascript
{
`import {
    getMessaging,
    getToken
} from "firebase/messaging";
const messaging = getMessaging();
getToken(messaging, {
    vapidKey: '<YOUR_PUBLIC_VAPID_KEY_HERE>'
}).then((currentToken) => {
    if (currentToken) {
      // Send the token to our server
      $Intk('sendFireBaseToken', currentToken); //<-- here
    } else {
      // ...
    }
}).catch((err) => {
    console.log('An error occurred while retrieving token. ', err);
    // ...
});`
}
```

### Synchronous

```javascript
{
`import {
    getMessaging,
    getToken
} from "firebase/messaging";
const messaging = getMessaging();
getToken(messaging, {
    vapidKey: '<YOUR_PUBLIC_VAPID_KEY_HERE>'
}).then((currentToken) => {
    if (currentToken) {
      // Send the token to our server
      $InTrack.sendFireBaseToken(currentToken); //<-- here
    } else {
      // ...
    }
}).catch((err) => {
    console.log('An error occurred while retrieving token. ', err);
    // ...
});`
}
```

**2- update your Service worker**

Add following code at the top of your current service worker:

```javascript
{
`if ('function' === typeof importScripts) {
    importScripts("https://static1.intrack.ir/api/web/download/sdk/v1/inTrack-sw.min.js");
}`
}
```

with this change, our service worker would handle incoming messages from inTrack, so you should leave them.
for example when you get new message in onBackgroundMessage method of Firebase SDK:

```javascript
{
`onBackgroundMessage((payload) => {
    if (payload?.data?.source === "$inTrack")
        return
    //...
});`
}
```

#### Integrate with other tools

If you generate your [VAPID](https://datatracker.ietf.org/doc/html/rfc8292) key and want **inTrack** to get user's permission, you need to give your public VAPID key to our SDK when you initializig it and our SDK will do the rest (getting user permission and sent pushSubscription to our servers).

but if you want to subscribe users yourself, or using a library that directly deal with browsers APIs, you need to give [PushSubscription](https://developer.mozilla.org/en-US/docs/Web/API/PushSubscription) object to our SDK by calling our `sendPushSubscription` method

### Asynchronous

```javascript
{
`$Intk('sendPushSubscription', token);`
}
```

### Synchronous

```javascript
{
`$InTrack.sendPushSubscription(token);`
}
```

**Example:**

### Asynchronous

```javascript
{
`navigator.serviceWorker.register('serviceworker.js');
navigator.serviceWorker.ready.then((serviceWorkerRegistration) => {
    const options = {
      userVisibleOnly: true,
      applicationServerKey,
    };
    serviceWorkerRegistration.pushManager.subscribe(options).then((pushSubscription) => {
      $Intk('sendPushSubscription', pushSubscription); //<-- here
    },
    (error) => {
      console.error(error);
    });
  });`
}
```

### Synchronous

```javascript
{
`navigator.serviceWorker.register('serviceworker.js');
navigator.serviceWorker.ready.then((serviceWorkerRegistration) => {
    const options = {
      userVisibleOnly: true,
      applicationServerKey,
    };
    serviceWorkerRegistration.pushManager.subscribe(options).then((pushSubscription) => {
      $InTrack.sendPushSubscription(pushSubscription); //<-- here
    },
    (error) => {
      console.error(error);
    });
  });`
}
```

On the other hand, you should add our service worker to your current service, by adding the following code at the top of it:

```javascript
{
`if ('function' === typeof importScripts) {
    importScripts("https://static1.intrack.ir/api/web/download/sdk/v1/inTrack-sw.min.js");
}`
}
```

with this change, our service worker would handle incoming messages from inTrack, so you should leave them.
for example when you get new message in your service worker:

```javascript
{
`self.addEventListener('push',
    function(i) {
      let data
      try {
        if (i.data) {
          data = JSON.parse(i.data.text())
        }
      } catch(error) {
        console.log('error', error)
      }
      if (data?.source === '$inTrack') return;
      //... rest of your code
    }
);`
}
```

:::info

You can checked push notification system events [here](../events#webpush-system-events)

:::
