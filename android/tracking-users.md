---
title: Tracking Users
version: 3.0.0+
source_path: android/tracking-users.md
docs_url: https://docs.intrack.ir/docs/getting-start/android/tracking-users
---
# Tracking Users

:::info

  We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will help you understand the workings of this section, better.

:::
## Identifying Users
At **inTrack** we start detecting users as soon as you [integrate your platform](android-getting-started) through our SDKs. Each time a user visits your App, the **inTrack** SDK automatically creates a unique ID `deviceID` for them. This helps us record users in our backend and create an anonymous profile. All their behavioral data ([System Events](../events.md#system-events), [Custom Events](../events.md#custom-events), [System User Attributes](tracking-users#user-system-attributes) and [Custom User Attributes](tracking-users#setting-custom-user-attributes)) are stored under the anonymous profile.

You can assign a unique ID `userID` to each user to identify them. We recommend that you assign a `userID` at any of the following moments in their lifeycle:

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

### Java

```java
{
  `$InTrack.recordLogin(UserDetails userDetails);
`
}
```



### kotlin

```kotlin
{
  `$InTrack.recordLogin(UserDetails())
`
}
```



Method recordLogin accepts a  `ir.$intrack.android.sdk.UserDetails`

### Logout

Make sure that you call logout when the user logs out and you don't want to attach any future event, session or user data with this user, until login is called again.

### Java

```java
{
  `$InTrack.recordLogout();
`
}
```



### kotlin

```kotlin
{
  `$InTrack.recordLogout()
`
}
```



### Profile Updates

In case of having any changes in user's details, these changes can be sent to **inTrack** server by calling the following method:

### Java

```java
{
  ` $InTrack.updateProfile(UserDetails userDetails);
`
}
```



### kotlin

```kotlin
{
  `$InTrack.updateProfile(UserDetails())
`
}
```



Method updateProfile accepts a `ir.android.sdk.UserDetails`

:::warning

In the update profile event, if any of the user detail keys are sent with a null value, the corresponding values previously set for the user will be deleted. This ensures that only the provided user details are updated, with existing values being removed if explicitly set to null.

:::

If the login event has not been called before updating the profile (i.e., the user is not logged in), or if the user does not have a user ID, the update profile method will update the corresponding anonymous user details. This ensures that user details can still be updated even if the user is not currently logged in or identified with a user ID.

## User Details(ir.android.sdk.UserDetails)

Several addtional details like a user's name. email address, location and so on can be associated with their user profile. All such details are referred to as User Attributes which can be of 2 types - System User Attributes and Custom User Attributes. (All user attributes are tracked for both, anonymous and known users.)

**inTrack** provides setters for assigning values against each attribute for your users. These attributes can be used to segment users, configure campaign targeting and personalize messages sent through each channel of engagement.

### User Details Attributes

#### User System Attributes

| Name         | Type          | Description                                                                                                                                                                                                          |
|--------------|---------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| userId       | String        | must only contain Alphanumeric characters and hyphens (- _).                                                                                                                                                         |
| firstName	   | String        | must be less than 1000 character.                                                                                                                                                                                    |
| lastName     | String        | must be less than 1000 character.                                                                                                                                                                                    |
| email        | String        | must be a valid email address.                                                                                                                                                                                       |
| phone        | String        | is a string with country code, for example: +989121234567(E164 format)                                                                                                                                               |
| country      | String        | must be less than 255 character.                                                                                                                                                                                     |
| state        | String        | must be less than 255 character.                                                                                                                                                                                     |
| city         | String        | must be less than 255 character.                                                                                                                                                                                     |
| gender       | ApiUserGender | System Attributes. `ir.$intrack.android.sdk.ApiUserGender`                                                                                                                           |
| birthday     | Date          | System Attributes.                                                                                                                                                                                                   |
| company      | String        | System Attributes.                                                                                                                                                                                                   |
| hashedPhone  | String        | Many businesses are averse to sharing contact details of their users with third-party platforms like **inTrack**. This is why, we've made it possible for you to pass encrypted phone of your users. |
| hashedEmail  | String        | Many businesses are averse to sharing contact details of their users with third-party platforms like **inTrack**. This is why, we've made it possible for you to pass encrypted email of your users. |
| smsOptIn     | Boolean       |                                                                                                                                                                                                                      |
| emailOptIn   | Boolean       |                                                                                                                                                                                                                      |
| pushOptIn    | Boolean       |                                                                                                                                                                                                                      |
| webPushOptIn | Boolean       |                                                                                                                                                                                                                      |

:::warning

That `userId` field is mandatory and you must always provide a string with less than 100 charachter.

:::

#### Setting Custom User Attributes

You can use the setUserAttributes method of our Android SDK to track custom attributes for a user. You can define `userAttributes` as a `HashMap`. each key of userAttributes must be less than 50 character.

### Sample of login event with user details

### Java

```java
{
  `UserDetails details = new UserDetails();
   details.setUserId("xxx-xxx");
   HashMap<String, Object> userAttributes = new HashMap<>();
   userAttributes.put("key", "value");
   details.setUserAttributes(userAttributes);
   $InTrack.recordLogin(details);
`
}
```



### kotlin

```kotlin
{
  `val details = UserDetails()
   details.userId = "xxx-xxx"
   val userAttributes = HashMap<String, Any>()
   userAttributes.set("key", "value")
   details.userAttributes = userAttributes
   $InTrack.recordLogin(details)
`
}
```



## Other methods

#### Getting device/anonymous ID

For tracking unknown users, **inTrack** SDK automatically generate a random id for each device. we refer this identifier as deviceId or anonymousId. you can get that by calling our getDeviceId method.

### Java

```java
{
  `String deviceId = $InTrack.getDeviceId();
`
}
```



### kotlin

```kotlin
{
  `val deviceId: String = $InTrack.getDeviceId()
`
}
```



#### Getting User ID

For tracking known users you can get them by calling our getUserId method.

### Java

```java
{
  `String deviceId = $InTrack.getUserId();
`
}
```



### kotlin

```kotlin
{
  `val deviceId: String = $InTrack.getUserId()
`
}
```
