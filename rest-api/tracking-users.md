---
title: Tracking Users
version: 3.0.0+
source_path: rest-api/tracking-users.md
docs_url: https://docs.intrack.ir/docs/getting-start/rest-api/tracking-users
---
# Tracking Users

:::info

  We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will help you understand the workings of this section, better.

:::

## Identifying Users

At **inTrack** we start detecting users as soon as you [integrate your platform](rest-api-getting-started). It is important to send `anonymousId` or `userId` in request. This helps us record users in our backend and create a profile. All their behavioral data ([SystemEvents](../events#system-events), [CustomEvents](../events#custom-events), System User Attributes and Custom User Attributes) are stored under the anonymous or known profile.
Assigning a unique userId helps collect the user's activity and information across systems in a single unified profile. Once the user profile's ID is assigned, it cannot be changed. An ID could be any String that uniquely identifies users in your system. However, we recommend using system-generated user IDs from your database instead of information that could change over time such as email address, username, or phone number.

You can assign:

:::warning

Anonymous ID by passing the parameter, anonymousID in the request body described below.

:::

:::warning

User id passing the parameter, userId in the request body described below.

:::

We recommend that you assign a `userID` at any of the following moments in their lifecycle:

* On signup page

* On login.

* On update profile.

* On pages where user identity becomes known

* When the user context changes.

Whenever a `userID` is assigned to a user it means that:

* The user is identified (they become Known Users in your [dashboard](https://dash.intrack.ir)).
* A new, Known User Profile is created for them that contains all their data.
* All their previous anonymous profiles are merged with the new Known User Profile to create a single unified view of your users. (This means that data from their first visit to your website to their latest interactions can all be found under a single user profile!)
* Once assigned user ID, a user ID cannot be changed.

:::info

  **inTrack** supported maximum 10 device per `userID`, it means each user id assigned to maximum 10 device in each type.

:::

:::info

  **inTrack** prefer to get user details with [RESTAPI](tracking-users.md). Rest API allow marketing to have user details before integration, also make update easier.

:::

## User Attributes

Several details like a user's name, email address, location, and so on can be associated with their user profile. All such details are referred to as User Attributes which can be of 2 types - System User Attributes and Custom User Attributes. (All user attributes are tracked for both, anonymous and known users.)
These attributes can be used to segment users, configure campaign targeting, and personalize messages sent through each channel of engagement.

Each session has its own user attributes that get copied from one session to the next. This is in contrast to event parameters, which may take on different values in each session. For this reason, we recommend that you use user attributes for recording details that don't change with each session or details with which you want the entire session to be associated.

#### Setting System Attributes

You can set system attributes by passing them as parameters in the request body described below.

#### Setting Custom Attributes

Custom Attributes enable you to understand a user's preferences in the context of your business and deliver hyper-personalized experiences in real-time.
You can set the value of Custom Attributes by passing them as key-value pairs in the attributes parameter in the request body described below.

**Here are a few things to keep in mind:**

* User Attribute names are case-sensitive and must be less than 50 characters long. String attribute values must be less than 1000 characters long. Additional characters will be truncated.

* You can create a maximum of 25 Custom User Attributes of each data type.

* UserAttributeName must not start with INT_. Names starting with INT_ are reserved exclusively for internal use at inTrack.

* The first datapoint synced to inTrack defines the data type for that user attribute. Thus, data types must be consistent with the value that you want to store against the attribute. If the data type is changed at a later date, then Custom Users Attribute data will stop flowing to your [inTrackdashboard](https://dash.intrack.ir).

| Name         | Type          | Description                                                                                                                                                                                                                                                               |
|--------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| userId       | String        | must only contain Alphanumeric characters and hyphens (- _).                                                                                                                                                                                                              |
| anonymousId  | String        | anonymousId can be of maximum 100 characters.                                                                                                                                                                                                                             |
| firstName	   | String        | must be less than 1000 character.                                                                                                                                                                                                                                         |
| lastName     | String        | must be less than 1000 character.                                                                                                                                                                                                                                         |
| email        | String        | must be a valid email address.                                                                                                                                                                                                                                            |
| phone        | String        | is a string with country code, for example: +989121234567(E164 format)                                                                                                                                                                                                    |
| country      | String        | must be less than 255 character.                                                                                                                                                                                                                                          |
| state        | String        | must be less than 255 character.                                                                                                                                                                                                                                          |
| city         | String        | must be less than 255 character.                                                                                                                                                                                                                                          |
| gender       | ApiUserGender | Can be one of male, female or other.                                                                                                                                                                                                                                      |
| birthday     | Date          | System Attributes.                                                                                                                                                                                                                                                        |
| company      | String        | System Attributes.                                                                                                                                                                                                                                                        |
| hashedPhone  | String        | Many businesses are averse to sharing contact details of their users with third-party platforms like **inTrack**. This is why, we've made it possible for you to pass encrypted phone of your users.                                                      |
| hashedEmail  | String        | Many businesses are averse to sharing contact details of their users with third-party platforms like **inTrack**. This is why, we've made it possible for you to pass encrypted email of your users.                                                      |
| smsOptIn     | Boolean       |                                                                                                                                                                                                                                                                           |
| emailOptIn   | Boolean       |                                                                                                                                                                                                                                                                           |
| pushOptIn    | Boolean       |                                                                                                                                                                                                                                                                           |
| webPushOptIn | Boolean       |                                                                                                                                                                                                                                                                           |
| attributes   | Object        | Custom attributes of the user as key-value pairs. For example:`{ "isPaidUser": true, "userPlan": "Premium" }` These data types are allowed for custom attributes: String, Number, Boolean, Date, JSON Array JSON Object. JSON Object can contain one of these data types. |

:::warning

Either one of `userId` or `anonymousId` is mandatory, and you must always provide a string with less than 100 character. In case both IDs are sent, anonymousId will be ignored.

:::

## Sending User Event Request

There are two types of passing users to **inTrack**:

* Single requests

* Batch request(Sending a list of users)

Maximum size of the user list in batch requests can be 100.

### Single requests(/users)

**Method:** POST

**URL STRUCTURE:** `<HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/users`

**AUTHENTICATION:** `Authorization: Bearer <YOUR_API_KEY>`

#### Headers

| Name                                       | Description                                                                                                                                                                                                                                                                                      |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ```x-request-id:<HEX_STRING>```            | Replace `{HEX_STRING}` with a unique random hex string.If API re-attempts are being made, this tag stops propagation of duplicate events within a 4 hour window. Events with same ID re-attempted within 4 hours won't get duplicated if the previous HTTP request was able to ingest the event. |
| ```Authorization: Bearer <YOUR_API_KEY>``` | Replace `<YOUR_$INTRACK_API_KEY>` with your inTrack API KEY.                                                                                                                                                                                     |

#### Requests Example

```bash
{
`curl -X POST <HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/users \\
    --header 'Authorization: Bearer <YOUR_$INTRACK_API_KEY>' \\
    --header 'Content-Type: application/json' \\
    --data '{
      "userId": "str", // for your known users
      "anonymousId": "str", // for your unknown users
      "firstName": "str",
      "lastName": "str",
      "birthday": "1980-07-09T03:28:38+04:30",
      "gender": "male", // one of these: [male, female, other]
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
      }
    }'`
}
```

### Batch requests(/usersBatch)

**Method:** POST

**URL STRUCTURE:** `<HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/usersBatch`

**AUTHENTICATION:** `Authorization: Bearer <YOUR_API_KEY>`

#### Headers

| Name                                       | Description                                                                                                                                                                                                                                                                                      |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ```x-request-id:<HEX_STRING>```            | Replace `{HEX_STRING}` with a unique random hex string.If API re-attempts are being made, this tag stops propagation of duplicate events within a 4 hour window. Events with same ID re-attempted within 4 hours won't get duplicated if the previous HTTP request was able to ingest the event. |
| ```Authorization: Bearer <YOUR_API_KEY>``` | Replace `<YOUR_$INTRACK_API_KEY>` with your inTrack API KEY.                                                                                                                                                                                     |

:::note

Maximum size of the user list in batch requests can be 100.

:::

#### Requests Example

```bash
{
`curl -X POST "<HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/usersBatch" \\
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
          }
        },
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
          }
        }
      ]
  }'`
}
```
