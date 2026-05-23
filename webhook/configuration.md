---
title: How to Configure
version: 3.0.0+
source_path: webhook/configuration.md
docs_url: https://docs.intrack.ir/docs/getting-start/webhook/configuration
---
# How to Configure

You will be able to configure URLs for different events from the Webhooks section of your  [inTrack dashboard](https://dash.intrack.ir) -> setting-> webhook. Following are the webhooks you can configure.

There are four different type of webhooks.

## 1: Custom Event

This webhook is triggered whenever a custom event is received, regardless of the source (e.g., SDKs, REST API, etc.).

The webhook configuration allows you to specify which custom events should trigger this webhook, providing flexibility to tailor it to your specific needs.

To customize the list of custom events that trigger the webhook, navigate to the custom events.

Example Webhook Body:

```json
{
  `{
      "licenceCode": "<YOUR_$INTRACK_LICENSE_CODE>",
      "userId": "34561",
      "anonymousId": "2213c9a6-a4ec-4481-a2ba-af8d72793bc0",
      "eventName": "update_cart",
      "eventTime": "2021-09-21T10:30:00+0430",
      "type": "CUSTOM_EVENT",
      "communicationId": 920,
      "variationId": 1003,
      "journeyId": 50,
      "eventData": {
        // custom attributes of event
        "customString": "stringValue",
        "customBoolean": true,
        "customNumber": 123,
        "customDate": "2021-09-21T10:30:00+0430"
      }
    }
`
}
```

### Webhook Details:

**licenseCode:** Your unique **inTrack** license code.

**userId:** Identifier of the user associated with the event.

**anonymousId:** Anonymous identifier of the user (optional).

**eventName:** Name of the custom event triggered ("update_cart" in this example).

**eventData:** Custom attributes associated with the event, such as strings, booleans, numbers, and dates.

## 2: System Event

This webhook will be sent upon occurrence of each system event.

You will find definition of system events and its custom attributes here: [System Events](../events.md)

Example Webhook Body:

```json
{
  `{
      "licenceCode": "<YOUR_$INTRACK_LICENSE_CODE>",
      "userId": "34561",
      "anonymousId": "2213c9a6-a4ec-4481-a2ba-af8d72793bc0",
      "eventName": "[INT]_email_sent",
      "eventTime": "2021-09-21T10:30:00+0430",
      "type": "SYSTEM_EVENT",
      "communicationId": 920,
      "variationId": 1003,
      "journeyId": 50,
      "eventData": {
        // custom attributes of event
        "customString": "stringValue",
        "customBoolean": true,
        "customNumber": 123,
        "customDate": "2021-09-21T10:30:00+0430"
      }
  }
`
}
```

:::note
To include the Begin Session event in your webhooks, simply enable the Include Begin Session option.
:::

## 3: Journey

This webhook is triggered whenever a change occurs in a journey definition.

Event Names:

* `JOURNEY_EVENT_CREATED:` Triggered when a new journey is created.

* `JOURNEY_EVENT_UPDATED:` Triggered when an existing journey is updated.

* `JOURNEY_EVENT_LAUNCHED:` Triggered when a journey is launched.

* `JOURNEY_EVENT_PAUSED:` Triggered when a journey is paused.

* `JOURNEY_EVENT_STOPPED:` Triggered when a journey is stopped.

* `JOURNEY_EVENT_DELETED:` Triggered when a journey is deleted.

Example Webhook Body:

```json
{
  `{
      "licenceCode": "<YOUR_$INTRACK_LICENSE_CODE>",
      "eventName": "JOURNEY_EVENT_CREATED",
      "eventTime": "2021-09-21T10:30:00+0430",
      "type": "JOURNEY",
      "eventData": {
        "createdDate": "2021-09-21T10:30:00+0430",
        "modifiedDate": "2021-09-21T10:30:00+0430",
        "createdBy": "Hamed Alavi",
        "modifiedBy": "Hamed Alavi",
        "name": "Journey 10.2.3",
        "id": 1023,
        "status": "DRAFT", // one of: 'DRAFT', 'UPCOMING', 'RUNNING', 'ENDED', 'PAUSED', 'STOPPED'
        "tags": ["Adservice"]
      }
  }
`
}
```

## 3: Communication

This webhook is triggered whenever a change occurs in a communication definition. It notifies you of events related to communication management:

* `COMMUNICATION_EVENT_CREATED:` Triggered when a new communication is created.

* `COMMUNICATION_EVENT_UPDATED:` Signals updates made to an existing communication.

* `COMMUNICATION_EVENT_LAUNCHED:` Notifies when a communication is launched.

* `COMMUNICATION_EVENT_PAUSED:` Alerts when a communication is paused.

* `COMMUNICATION_EVENT_STOPPED:` Indicates when a communication is stopped.

* `COMMUNICATION_EVENT_DELETED:` Signals when a communication is deleted.

Example Webhook Body:

```json
{
  `{
      "licenceCode": "<YOUR_$INTRACK_LICENSE_CODE>",
      "eventName": "COMMUNICATION_EVENT_CREATED",
      "eventTime": "2021-09-21T10:30:00+0430",
      "type": "COMMUNICATION",
      "eventData": {
        "createdDate": "2021-09-21T10:30:00+0430",
        "modifiedDate": "2021-09-21T10:30:00+0430",
        "createdBy": "Hamed Alavi",
        "modifiedBy": "Hamed Alavi",
        "name": "Sending Test SMS",
        "id": 1023,
        "type": "ONE_TIME",
        "channel": "SMS",
        "status": "DRAFT",
        "tags": ["Adservice"]
      }
  }
`
}
```

### Webhook Details:

**licenceCode:** Your unique **inTrack** license code.

**eventName:** Name of the event that triggered the webhook (COMMUNICATION_EVENT_CREATED in this example).

**eventTime:** Timestamp of when the event occurred.

**eventData:** Detailed information about the communication event, including creation and modification details, name, ID, type, channel, status, and tags.

## 4: Batch Webooks:
Batch webhook responses will consist of an array containing multiple instances of the same objects listed above, such as communication, journey, and events.

```json
  [
    {
      // same objects as above
  }
]
```
