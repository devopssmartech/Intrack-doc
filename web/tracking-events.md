---
title: Tracking Events
version: 3.0.0+
source_path: web/tracking-events.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/web/tracking-events.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/web/tracking-events.md
---
# Tracking Events

:::info

We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their attributes before proceeding. Doing so will help you better understand the workings of this section.

:::

**inTrack** starts tracking some events as soon as you [integrateSDK](web-getting-started). These are called System Events and track some generic user interactions with your app and campaigns. Here's a [ListOfTheSystemEvents](../events#system-events) that are automatically tracked by us.

You can create Custom Events to track any other user interactions that are curcial for your business. Each Custom Event can further be defined by Event Attributes like price, quantity, category and so on. Such granular data enables you to engage users through highly contextual and personalized campaigns through all the channels of engagement.

## Tracking Custom Events

Users can create their own custom events and send them to **inTrack** server. This can be done by calling sendEvent with an input having the following model:

```JavaScript

{
`EventData: {
    eventName: String,
    eventData: JsonObject
}`
}
```

For example:

### Asynchronous

```javascript
{
  `$Intk('sendEvent', {
    eventName: 'my-custom-event',
    eventData: {
      count: 1000,
      label: 'two',
      accepted: false,
      date: '1970-01-01',
      myDetails: {
        one: 1,
        myList: ['firstValue', 'secondValue'],
      },
    },
  });`
}
```



### Synchronous

```javascript
{
  `$InTrack.sendEvent({
    eventName: 'my-custom-event',
    eventData: {
      count: 1000,
      label: 'two',
      accepted: false,
      date: '1970-01-01',
      myDetails: {
        one: 1,
        myList: ['firstValue', 'secondValue'],
      },
    },
});`
}
```



### Guidelines
Here are a few things to keep in mind:

* Event name must only contain Alphanumeric characters and hyphens (- _).

* Each key of eventDetails HashMap, must be less than 50 character (otherwise it will be removed by the SDK).

* Custom Event Attributes can be of these data types: String, all subclasses of Number, Boolean, Date.

* Date field should be in ISO-8601 format. (otherwise we detect is as String)

:::warning

The first datapoint synced to **inTrack** defines the data type for that event attribute. Thus, data types must be consistent with the value that you want to store against the attribute. If the data type is changed at a later date, then Custom Event Attribute data will stop flowing to your [inTrack dashboard](https://dash.intrack.ir).

:::

## Advance features

### Sending User ID with Events

To log in a user, you can send the userId to the sendEvent method. Below are examples of both asynchronous and synchronous usage:

### Asynchronous

```javascript
{
`$Intk('sendEvent', {
    userId: 'USER_ID',
    eventName: 'my-custom-event',
    eventData: {
      count: 1000,
      label: 'two',
      accepted: false,
      myDetails: {
        one: 1,
        myList: ['firstValue', 'secondValue'],
      },
    },
});`
}
```

### Synchronous

```javascript
{
`$InTrack.sendEvent({
    userId: 'USER_ID',
    eventName: 'my-custom-event',
    eventData: {
      count: 1000,
      label: 'two',
      accepted: false,
      myDetails: {
        one: 1,
        myList: ['firstValue', 'secondValue'],
      },
    },
});`
}
```



If the user is not already logged in, the SDK will first call the login method internally (sending a login event) before registering the custom event.
