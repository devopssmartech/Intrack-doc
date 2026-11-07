---
title: On-Site
version: 3.0.0+
source_path: web/on-site.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/web/on-site.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/web/on-site.md
---
# On-Site

## Initialization

Initializing On Site messages service is done by calling initOnSiteMessaging method with optional config parameter.

:::note
 that for using onSite you should use our latest sdk (https://static1.intrack.ir/api/web/download/sdk/v1/inTrack.min.js?v=00)
 :::



### Asynchronous

```javascript
{
  `$Intk('initOnSiteMessaging', config);
`
}
```



### Synchronous

```javascript
{
  `$InTrack.initOnSiteMessaging(config);
`
}
```



Method `initOnSiteMessaging` accepts a config parameter with the following model. You can customize look and feel of widget with these parameters:

```javascript
{
`config: {
    displayMessage: null,
    disableAutoTracking: false
}`
}
```

| Property            | Type     | Description                                                                        |
|---------------------|----------|------------------------------------------------------------------------------------|
| displayMessage      | function | Used for some businesses whose style of displaying pop-ups is different from ours. |
| disableAutoTracking | boolean  | Will disable our automatic page tracker.                                           |

## Page Tracking

for showing on site message to users in spefic pages, we need to track users navigation through your site, by default our SDK will do this if your routing system is compatible with browser's [History API](https://developer.mozilla.org/en-US/docs/Web/API/History_API) or using some library that use that behind the scene. if you want to disable this feature, you can set `disableAutoTracking` to true, in config options.

generally for on site campaigns we need to track page's title and store

### Automatic

if you don't disable our automatic tracking, we will count user's page view and keep track of the page title and page state along with `href`, `url Path`, `Query String` and `Fragmant identifier` (as page data)

our tracking object is look like this:

```javascript
{
`pageInfo: {
    title: "", // document.title
    data: {
      ...history.state,
      _int_href:"", //location.href
      _int_pathname:"", //location.pathname
      _int_hash:"", //location.hash
      _int_search:"" //location.search
    }
}`
}
```

### Manual tracking

you can tell our SDK about user's page views manually, by calling `sendScreenInfo` method.



### Asynchronous

```javascript
{
  `$Intk('sendScreenInfo', pageInfo, replace);`
}
```

### Synchronous

```javascript
{
  `$InTrack.sendScreenInfo(pageInfo, replace);`
}
```



| Property  | Type    | Description                                                                                                             |
|-----------|---------|-------------------------------------------------------------------------------------------------------------------------|
| pageInfo  | Object  | It should contain two keys: title and data. title is a string representing the page's title and data is an object that could contain any key that matters to you. |
| replace   | boolean | If it is set to true, we will count this page. It is like replaceState.                                                 |

you can use `sendScreenInfo` with our automatic page tracking for enriching page data. for example you want to display a message to a user who is seeing your product page, and whose price is more that a specific number, in this case you could call this method with following properties:



### Asynchronous


```javascript
        {
          `$Intk('sendScreenInfo', title: 'MY_PRODUCT', data: {price: 10000}, false);`
        }
```




### Synchronous


```javascript
        {
          `$InTrack.sendScreenInfo(title: 'MY_PRODUCT', data: {price: 10000}, false);`
        }
```



### Custom Display Message

if you are not happy with the way that we display messages, you could handle that yourself. for this, you must provide a function to `displayMessage` key of config:

### Asynchronous

```javascript
{
  `$Intk('initOnSiteMessaging',
    displayMessage: (data) => {
      console.log('message data', data);
      //display message here...
    }
  );`
}
```

### Synchronous

```javascript
{
  `$InTrack.initOnSiteMessaging(
    displayMessage: (data) => {
      console.log('message data', data);
      //display message here...
    }
  );`
}
```

by doing this, we track user's activity and when campaing conditions are met, we will call your provided function and send message information to you.

Depending on message layout type(`Text & Link`, `Coupon` and `Image only`) You wil receive the following data:

### Coupon

```json
{
  "id": "",
  "layout": "COUPON",
  "title": "",
  "description": "",
  "couponCode": "",
  "image": "",
  "theme": {
    "bg_color": "",
    "title_color": "",
    "description_color": "",
    "border_radius": "",
    "cta_background_coupon": "",
    "cta_font_color_coupon`": "",
  },
  "showCount": 1,
  "ttl": 1,
}
```

### Text & Link

```json
{
  "id": "",
  "layout": "TEXT",
  "label": "",
  "title": "",
  "description": "",
  "image": "",
  "primaryButtonLabel": "",
  "primaryButtonOnClick": "",
  "theme": {
    "bg_color": "",
    "label_color": "",
    "title_color": "",
    "description_color": "",
    "border_radius": "",
    "cta_background": "",
    "cta_font_color": "",
  },
  "showCount": 1,
  "ttl": 1,
}
```

### Image only

```json
  {
  "id": "",
  "layout": "IMAGE",
  "image": "",
  "primaryButtonOnClick": "",
  "theme": {
    "border_radius": "",
  },
  "showCount": 1,
  "ttl": 1,
}
```


after you displaying message to user, you should tell us about user's activity about a message with sending events:

if user perform primary action of message (for example by clicking on action buttons) send `ON_SITE_CLICK` event:

### Asynchronous

```javascript
{
  `$Intk('sendEvent', {
    eventName: 'ON_SITE_CLICK',
    eventData: {
      id: '', //id of the message
    },
    isSystem: true
  });`
}
```

### Synchronous

```javascript
{
  `$InTrack.sendEvent({
    eventName: 'ON_SITE_CLICK',
    eventData: {
      id: '', //id of the message
    },
    isSystem: true
  });`
}
```

and if user dissmissed the message send `ON_SITE_CLOSED` event:

### Asynchronous

```javascript
{
  `$Intk('sendEvent', {
    eventName: 'ON_SITE_CLOSED',
    eventData: {
      id: '', //id of the message
    },
    isSystem: true
  });`
}
```

### Synchronous

```javascript
{
  `$InTrack.sendEvent({
    eventName: 'ON_SITE_CLOSED',
    eventData: {
      id: '', //id of the message
    },
    isSystem: true
  });`
}
```

note that in these cases you should set `isSystem` key to true.

:::info

You can checked Onsite system events [here](../events#onsite-system-events)

:::
