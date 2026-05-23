---
title: Tracking Users
version: 3.0.0+
source_path: flutter/tracking-users.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/flutter/tracking-users.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/flutter/tracking-users.md
---
# Tracking Users

:::info

  We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will help you understand the workings of this section, better.

:::

## Identifying Users
At **inTrack** we start detecting users as soon as you [integrate your platform](flutter-getting-started.md) through our SDKs. Each time a user visits your App, the **inTrack** SDK automatically creates a unique ID `deviceID` for them. This helps us record users in our backend and create an anonymous profile. All their behavioral data ([SystemEvents](../events#system-events), [CustomEvents](../events#custom-events), [System User Attributes](tracking-users#user-system-attributes) and Custom User Attributes) are stored under the anonymous profile.

ou can assign a unique ID `userID` to each user to identify them. We recommend that you assign a `userID` at any of the following moments in their lifeycle:

On login.

On update profile.

Whenever a `userID` is assigned to a user it means that:

* The user is identified (they become Known Users in your [dashboard](https://dash.intrack.ir)).
* A new, Known User Profile is created for them that contains all their data.
* All their previous anonymous profiles are merged with the new Known User Profile to create a single unified view of your users. (This means that data from their first visit to your website to their latest interactions can all be found under a single user profile!)

:::info

  **inTrack** supported maximum 10 device per `userID`, it means each user id assigned to maximum 10 device in each type.

:::

:::note Important
The ability to send/update user attributes using SDKs is disabled by default and sdk only can create an empty user profile if the profile does not already exist.
To enable this feature, please contact our support team and request activation for your project.

⚠️ Note: We always recommend to use our REST API for sending/updating user profile, because it is more flexible and secure way to manipulate user profiles and make the update easier. also Rest API allows your marketing team to have user details before sdk integration.
:::

### Login

You can assign an ID by calling the login method. All attributes, events and session information accumulated before this API has been called get associated to an anonymous user created by default.

Once login is called, all of this stored information is attributed to this identified user.

Make sure you call login as soon as the user logs in to your application, or whenever earliest you are able to identify the user.

```dart
{
  `$InTrack.recordLogin(userDetails);
`
}
```

Login accepts an object with the following structure:

```dart
UserDetails: {
    firstName: String,
    lastName: String,
    email: String,
    phone: String,
    country: String,
    state: String,
    city: String,
    gender: "male" | "female" | "other",
    birthday: String, //ISO-8601
    company: String,
    hashedPhone: String,
    hashedEmail: String,
    smsOptIn: Boolean,
    emailOptIn: Boolean,
    pushOptIn: Boolean,
    webPushOptIn: Boolean,
    userId: String,
    attributes: Object
}
```

sample of login event with attributes:

```dart
{
  `const customData = {
      'string': 'string',
      'boolean': true,
      'integer': 100000,
      'double': 19.99,
      'float': 5.75,
      'date': '1989-07-31T09:27:37Z',
    };

    const data = {
      'firstName': 'firstname',
      'lastName': 'lastname',
      'email': 'test@test.com',
      'phone': '+22000000',
      'country': 'Iran',
      'state': 'Tehran',
      'city': 'Tehran',
      'gender': 'male',
      'birthday': '1989-07-31T09:27:37Z', // ISO-8601
      'company': 'company',
      'userId': 'UID22222222',
      'attributes': customData,
    };

    $InTrack.recordLogin(data);
`
}
```

### Logout

Make sure that you call logout when the user logs out and you don't want to attach any future event, session or user data with this user, until login is called again.

```dart
{
  `$InTrack.logout();
`
}
```

### Profile Updates

In case of having any changes in user's details, these changes can be sent to **inTrack** server by calling the following method:

```dart
{
  `$InTrack.updateProfile(userDetails);
`
}
```

Profile Updates accepts an object like login object.

sample of Profile Updates event with attributes:

```dart
{
  `const data = {
    'firstName': 'firstname_changed',
    'lastName': 'lastname_changed',
    }

  $InTrack.updateProfile(data);
`
}
```

:::warning

In the update profile event, if any of the user detail keys are sent with a null value, the corresponding values previously set for the user will be deleted. This ensures that only the provided user details are updated, with existing values being removed if explicitly set to null.

:::

If the login event has not been called before updating the profile (i.e., the user is not logged in), or if the user does not have a user ID, the update profile method will update the corresponding anonymous user details. This ensures that user details can still be updated even if the user is not currently logged in or identified with a user ID.

#### User System Attributes

**Checked below table to know more about user detail object**.

| Name | Type  | Description       |
|-------------------|------------------|-------------------|
|userId|String|must only contain Alphanumeric characters and hyphens (- _).|
| firstName	 | String | must be less than 1000 character.|
|lastName|String|must be less than 1000 character.|
|email|String|must be a valid email address.|
|phone|String|is a string with country code, for example: +989121234567(E164 format)|
|country|String|must be less than 255 character.|
|state|String|must be less than 255 character.|
|city|String|must be less than 255 character.|
|gender|String| male", "female" ,"other"|
|birthday|Date|ISO-8601|
|company|String||
|hashedPhone|String|Many businesses are averse to sharing contact details of their users with third-party platforms like **inTrack**. This is why, we've made it possible for you to pass encrypted phone of your users.|
|hashedEmail|String|Many businesses are averse to sharing contact details of their users with third-party platforms like **inTrack**. This is why, we've made it possible for you to pass encrypted email of your users.|
|attributes|object|each key of userAttributes must be less than 50 character.|
|smsOptIn|Boolean||
|emailOptIn|Boolean||
|pushOptIn|Boolean||
|webPushOptIn|Boolean||

:::warning

  That `userId` field is mandatory and you must always provide a string with less than 100 charachter.

:::

## Other methods

#### Getting device/anonymous ID

For tracking unknown users, **inTrack** SDK automatically generate a random id for each device. we refer this identifier as deviceId or anonymousId. you can get that by calling our `getDeviceId ` method.

```dart
{
  `String? deviceId = await $InTrack.getDeviceId();
`
}
```

#### Getting User ID

For tracking known users you can get them by calling our getUserId method.

```dart
{
  `String? userId = await $InTrack.getUserId();
`
}
```
