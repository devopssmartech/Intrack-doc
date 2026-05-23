---
title: Tracking Events
version: 3.0.0+
source_path: rest-api/tracking-events.md
docs_url: https://docs.intrack.ir/docs/getting-start/rest-api/tracking-events
---
# Tracking Events

:::info

  We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their attributes before proceeding. Doing so will help you better understand the workings of this section.

:::

**inTrack** offers API for tracking Custom Events through the `/events` endpoint described below.

## System Events

**inTrack** starts tracking some events automatically as soon as you integrated. These are called [SystemEvents](../events#system-events), and they track your users’ interactions with your apps, website, and campaigns. [SystemEvents](../events#system-events) make it easier to analyze user behavior and optimize your marketing around common business goals such as driving user registrations or purchases. You can use custom events for any other user actions you want to track.

You cannot create system events using **inTrack** API.

Using **inTrack** API, you should avoid creating events with the same event name as that of system events to avoid confusion.

## Custom Events

You can create [CustomEvents](../events#custom-events) to track any other user interactions that are crucial for your business. Each [CustomEvents](../events#custom-events) can further be defined by Event Attributes like price, quantity, category and so on. Such granular data enables you to engage users through highly contextual and personalized campaigns through all the channels of engagement.

### Guidelines

Here are a few things to keep in mind:

* Event name must only contain Alphanumeric characters and hyphens (- _).

* [CustomEvents](../events#custom-events) and Custom Event Attribute names are case-sensitive and must be less than 50 characters long. String attribute values must be less than 1000 characters long.

* Custom Event Attributes can be of these data types: String, all subclasses of Number, Boolean, Date, JSON Array, JSON Object. JSON Object can contain only one of these data types

* Date field should be in ISO-8601 format. (otherwise we detect is as String)

* Maximum size of a value for an attribute is 4 KB, and it will be dropped in case of passing the threshold.

* If an event is bigger than 8 KB (after pruning too large attributes) it won't be processed and will be dropped.

* eventName or eventAttributeName must not start with INT_. Names starting withINT_ are reserved exclusively for internal use at **inTrack**.

* The first datapoint synced to **inTrack** defines the data type for that event attribute. Thus, data types must be consistent with the value that you want to store against the attribute. If the data type is changed at a later date, then Custom Event Attribute data will stop flowing to your [inTrack dashboard](https://dash.intrack.ir).

* You can create a maximum of 25 Event Attributes of each data type for a Custom Event. (i.e. 25 attributes of Number data type, 25 attributes of String, Date, and Boolean data type).

### Event Attributes

| Name        | Type   | Description                                                                                                           |
|-------------|--------|-----------------------------------------------------------------------------------------------------------------------|
| userId      | String | must only contain Alphanumeric characters and hyphens (- _).                                                          |
| anonymousId | String | anonymousId can be of maximum 100 characters.                                                                         |
| eventName   | String | Name of the event.                                                                                                    |
| eventTime   | String | Date and time when the event occurred in ISO format: yyyy-MM-ddTHH:mm:ss±hhmm.                                        |
| eventId     | String | Unique identifier for event. By sending this you can prevent processing events with same name and id for certain user |

:::warning

Either one of `userId` or `anonymousId` is mandatory, and you must always provide a string with less than 100 characters. In case both IDs are sent, anonymousId will be ignored.

:::

:::warning

Sending `eventName` is mandatory.

:::

Highly recommend that you create an Excel sheet to list down:
* The Custom Events you want to track.
* The corresponding Custom Attributes of each Event.
* The data type of the values you will be tracking for each Custom Attribute.

## Sending Event Request

There are two types of passing event to **inTrack**:

* Single requests

* Batch request(Sending a list of events)

Maximum size of the event list in batch requests can be 100.

### Single requests(/events)

**Method:** POST

**URL STRUCTURE:** `<HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/events`

**AUTHENTICATION:** `Authorization: Bearer <YOUR_API_KEY>`

#### Headers

| Name                                       | Description                                                                                                                                                                                                                                                                                      |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ```x-request-id:<HEX_STRING>```            | Replace `{HEX_STRING}` with a unique random hex string.If API re-attempts are being made, this tag stops propagation of duplicate events within a 4 hour window. Events with same ID re-attempted within 4 hours won't get duplicated if the previous HTTP request was able to ingest the event. |
| ```Authorization: Bearer <YOUR_API_KEY>``` | Replace `<YOUR_$INTRACK_API_KEY>` with your **inTrack** API KEY.                                                                                                                                                                                 |

#### Requests Example

```bash
{
`curl -X POST <HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/events  \\
    --header 'Authorization: Bearer <YOUR_$INTRACK_API_KEY>' \\
    --header 'Content-Type: application/json' \\
    --data '{
      "userId": "xxx-xxx", // for your known users
      "anonymousId": "yyy-yyy",// for your unknown users
      "firstName": "firstname",
      "lastName": "lastname",
      "birthday": "1980-07-09T03:28:38+04:30",
      "gender": "male",  // one of these: [male, female, other]
      "email": "user@email.com",
      "emailOptIn": false,
      "phone": "+989121234567",
      "hashedPhone": "+98********67",
      "hashedEmail": "u1******.com",
      "smsOptIn": false,
      "pushOptIn": false,
      "webPushOptIn": false,
      "company": "str",
      "country": "str",
      "city": "str",
      "state": "str",
      "attributes": {
        "additionalProp1": "value1",
        "additionalProp2": 123.4,
        "additionalProp3": "1988-11-11T00:00:00+0000",
        "field4": true
      },
      "eventName": "str",
      "eventData": {
        "additionalProp1": "STRING",
        "additionalProp2": true,
        "additionalProp3": "2021-07-09T03:28:38+04:30"
      },
      "eventTime": "2021-07-09T03:28:38+04:30"
    }'`
}
```

### Batch requests(/eventsBatch)

**Method:** POST

**URL STRUCTURE:** `<HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/eventsBatch`

**AUTHENTICATION:** `Authorization: Bearer <YOUR_API_KEY>`

#### Headers

| Name                                       | Description                                                                                                                                                                                                                                                                                      |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ```x-request-id:<HEX_STRING>```            | Replace `{HEX_STRING}` with a unique random hex string.If API re-attempts are being made, this tag stops propagation of duplicate events within a 4 hour window. Events with same ID re-attempted within 4 hours won't get duplicated if the previous HTTP request was able to ingest the event. |
| ```Authorization: Bearer <YOUR_API_KEY>``` | Replace `<YOUR_$INTRACK_API_KEY>` with your **inTrack** API KEY.                                                                                                                                                                                 |

:::note

Maximum size of the event list in batch requests can be 100.

:::

#### Request Example

```bash
{
`curl -X POST "<HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/eventsBatch" \\
    --header "Authorization: Bearer <YOUR_$INTRACK_API_KEY>" \\
    --header "Content-Type: application/json" \\
    --data '{
      "list": [
        {
          "userId": "str",
          "anonymousId": "str",
          "firstName": "str",
          "lastName": "str",
          "birthday": "1980-07-09T03:28:38+04:30",
          "gender": "male",
          "email": "user@email.com",
          "emailOptIn": false,
          "phone": "+989121234567",
          "hashedPhone": "+98********67",
          "hashedEmail": "u1******.com",
          "smsOptIn": false,
          "pushOptIn": false,
          "webPushOptIn": false,
          "company": "str",
          "country": "str",
          "city": "str",
          "state": "str",
          "attributes": {
            "additionalProp1": "value1",
            "additionalProp2": 123.4,
            "additionalProp3": "1988-11-11T00:00:00+0000",
            "field4": true
          },
          "eventName": "str",
          "eventData": {
            "additionalProp1": "STRING",
            "additionalProp2": true,
            "additionalProp3": "2021-07-09T03:28:38+04:30"
          },
          "eventTime": "2021-07-09T03:28:38+04:30"
        },
        {
          "userId": "str2",
          "anonymousId": "str2",
          "firstName": "str",
          "lastName": "str",
          "birthday": "1980-07-09T03:28:38+04:30",
          "gender": "male",
          "email": "user@email.com",
          "emailOptIn": false,
          "phone": "+989121234567",
          "hashedPhone": "+98********67",
          "hashedEmail": "u1******.com",
          "smsOptIn": false,
          "pushOptIn": false,
          "webPushOptIn": false,
          "company": "str",
          "country": "str",
          "city": "str",
          "state": "str",
          "attributes": {
            "additionalProp1": "value1",
            "additionalProp2": 123.4,
            "additionalProp3": "1988-11-11T00:00:00+0000",
            "field4": true
          },
          "eventName": "str",
          "eventData": {
            "additionalProp1": "STRING",
            "additionalProp2": true,
            "additionalProp3": "2021-07-09T03:28:38+04:30"
          },
          "eventTime": "2021-07-09T03:28:38+04:30"
        }
      ]
    }'`
}
```
