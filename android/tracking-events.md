---
title: Tracking Events
version: 3.0.0+
source_path: android/tracking-events.md
docs_url: https://docs.intrack.ir/docs/getting-start/android/tracking-events
---
# Tracking Events

:::info

We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their attributes before proceeding. Doing so will help you better understand the workings of this section.

:::

**inTrack** starts tracking some events as soon as you [integrateSDK](android-getting-started). These are called System Events and track some generic user interactions with your app and campaigns. Here's a [ListOfTheSystemEvents](../events#system-events) that are automatically tracked by us.

You can create Custom Events to track any other user interactions that are curcial for your business. Each Custom Event can further be defined by Event Attributes like price, quantity, category and so on. Such granular data enables you to engage users through highly contextual and personalized campaigns through all the channels of engagement.

## Tracking Custom Events

Users can create their own custom events and send them to inTrack server. This can be done by calling `recordEvent`, For example:

### Java

```java
{
  `HashMap<String, Object> eventDetails = new HashMap<String, Object>();
   eventDetails.put("currency", "IRR");
   eventDetails.put("app_version", "1.0");
   $InTrack.recordEvent("custom_event_name", eventDetails);
`
}
```



### kotlin

```kotlin
{
  `val eventDetails = HashMap<String, Any>()
  eventDetails["currency"] = "IRR"
  eventDetails["app_version"] = "1.0"
  $InTrack.recordEvent("custom_event_name", eventDetails)
`
}
```



### Guidelines
Here are a few things to keep in mind:

* Event name must only contain Alphanumeric characters and hyphens (- _).

* Each key of eventDetails HashMap, must be less than 50 character (otherwise it will be removed by the SDK).

* Custom Event Attributes can be of these data types: String, all subclasses of Number, Boolean, Date.

* Date field should be in ISO-8601 format. (otherwise we detect is as String)

:::warning

:warning: The first datapoint synced to **inTrack** defines the data type for that event attribute. Thus, data types must be consistent with the value that you want to store against the attribute. If the data type is changed at a later date, then Custom Event Attribute data will stop flowing to your **inTrack** dashboard.

:::

## Other methods

### Sending Custom-Channel events

If you communicate to your app through our Custom-Channel (for example if you have internal messaging service), you could send Delivery and Click evnets of those campaigns with the following methods :

sending delivery event:

### Java

```java
{
  `$InTrack.recordCustomChannelDelivery(applicationContext, messageToken);
`
}
```



### kotlin

```kotlin
{
  `$InTrack.recordCustomChannelDelivery(applicationContext, messageToken)
`
}
```



sending click event:

### Java

```java
{
  `$InTrack.recordCustomChannelClick(applicationContext,messageToken);
`
}
```



### kotlin

```kotlin
{
  `$InTrack.recordCustomChannelClick(applicationContext,messageToken)
`
}
```




in both methods, messageToken is the unique token identifier for messages that generate by inTrack. please refer to our custom-channel documents.
