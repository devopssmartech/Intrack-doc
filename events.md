---
title: Events
version: 3.0.0+
source_path: events.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/events.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/events.md
---
# Events
All behavioral data points are called Events in [your dashboard](https://dash.intrack.ir). And each Event can further be understood in the context of its Attributes which includes details like time, location, device details, price, quantity, and so on.

:::info

The term Event refers to all the actions performed by users while interacting with your mobile apps, website, and campaigns.

:::

This enables you to gain in-depth insights into user interactions across your app, website, and channels. Leveraging this data allows you to segment users, personalize messages, and configure campaign targeting effectively.
## Events Classification:

*  **System Events:**
System Events are pre-defined by **inTrack** and automatically tracked for platforms post-integration. These events capture standard user interactions that are essential for understanding user behavior across different platforms.

* **Custom Events:**
Custom Events are defined by you for each platform and tracked through the respective SDKs.

Let's walk you through this.

## System Events
We have pre-defined several generic actions that users can perform while interacting with your app, website and campaigns. These actions are referred to as System Event and are automatically tracked for your platforms once you integrate them with your **inTrack** account.
Here's a list of all the System Events that are automatically tracked for all your users post integration:

### OnSite System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
|ON_SITE_SENT|[INT]_ON_SITE_SENT|Sent onsite from panel|
|ON_SITE_ACCEPTED|[INT]_ON_SITE_ACCEPTED|Accepted by provider|
|ON_SITE_QUEUED|[INT]_ON_SITE_QUEUED|Having onsite queued to be sent to the user|
|ON_SITE_RECEIVED|[INT]_ON_SITE_RECEIVED|Having onsite delivered to the user's device|
|ON_SITE_DELIVERED|[INT]_ON_SITE_DELIVERED|display onsite to the user|
|ON_SITE_CLICK|[INT]_ON_SITE_CLICK|Clicking a button in onsite body|
|ON_SITE_CLOSED|[INT]_ON_SITE_CLOSED|Close onsite by clicking close button|
|ON_SITE_CONTROL_GROUP|[INT]_ON_SITE_CONTROL_GROUP|When someone falls into the control group |
|ON_SITE_REJECTED|[INT]_ON_SITE_REJECTED|When onsite is rejected by provider|
|ON_SITE_VIEW_FAILED|[INT]_ON_SITE_VIEW_FAILED|Failed to render view of onsite|

### InApp System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
|IN_APP_SENT|[INT]_IN_APP_SENT|Sent in-app from panel|
|IN_APP_ACCEPTED|[INT]_IN_APP_ACCEPTED|Accepted by provider|
|IN_APP_QUEUED|[INT]_IN_APP_QUEUED|Having in-app queued to be sent to the user|
|IN_APP_RECEIVED|[INT]_IN_APP_RECEIVED|Having in-app delivered to the user's device|
|IN_APP_DELIVERED|[INT]_IN_APP_DELIVERED|display in-app to the user|
|IN_APP_CLICK|[INT]_IN_APP_CLICK|Clicking a button in in-app body|
|IN_APP_CLOSED|[INT]_IN_APP_CLOSED|Close in-app by clicking close button|
|IN_APP_CONTROL_GROUP|[INT]IN_APP_CONTROL_GROUP|When someone falls into the control group|
|IN_APP_REJECTED|[INT]_IN_APP_REJECTED|When in-app is rejected by provider|
|IN_APP_VIEW_FAILED|[INT]_IN_APP_VIEW_FAILED|Failed to render view of in-app|
|IN_APP_FAILED|[INT]_IN_APP_FAILED|Failed to load in-app|

### Push System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
| FCM_REGISTERED | [INT]_fcm_registered | Registering to push server |
| PUSH_NOTIFICATION_SENT | [INT]_push_notification_sent | Sending push notification |
| PUSH_NOTIFICATION_ACCEPTED | [INT]_push_notification_accepted | Receiving message id from push server |
| PUSH_NOTIFICATION_CLICK | [INT]_push_notification_click | When a user clicks a Mobile Push Notification or Web Push Notification. |
| PUSH_NOTIFICATION_CONTROL_GROUP | [INT]_push_notification_control_group | Having push assigned to control group |
| PUSH_NOTIFICATION_QUEUED | [INT]_push_notification_queued | Having push queued to be sent to the user |
| PUSH_NOTIFICATION_REJECTED | [INT]_push_notification_rejected | When a Mobile Push Notification is rejected by FCM |
| PUSH_NOTIFICATION_RECEIVED | [INT]_push_notification_received | Having push delivered to the user |

### WebPush System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
| WEB_PUSH_REGISTERED | [INT]_web_push_registered | Registering to web push server |
| WEB_PUSH_SENT | [INT]_web_push_sent |  Sending web push |
| WEB_PUSH_ACCEPTED | [INT]_web_push_accepted | Receiving message id from web push server |
| WEB_PUSH_CLICK | [INT]_web_push_click |  Clicking a button in web push |
| WEB_PUSH_CONTROL_GROUP | [INT]_web_push_control_group | Having web push assigned to control group |
| WEB_PUSH_QUEUED | [INT]_web_push_queued | Having web push queued to be sent to the user |
| WEB_PUSH_RECEIVED | [INT]_web_push_received | Having web push delivered to the user |
| WEB_PUSH_REJECTED | [INT]_web_push_rejected | Rejection of web push by web push server |
| WEB_PUSH_UNREGISTERED | [INT]_web_push_unregistered | Unregistering from web push server |
| WEB_PUSH_EXPIRED | [INT]_web_push_expired | web push is expired |

### Application System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
| APP_INSTALLED | [INT]_app_installed | Installation of app on a new device |
| APP_UNINSTALLED | [INT]_app_uninstalled | Uninstalling app |
| APP_CRASHED|[INT]_app_crashed|Receipt of app crash from SDK|
| APP_UPDATED|[INT]_app_updated|When the user updates the app|

### Email System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
| EMAIL_SENT| [INT]_email_sent | Sending email to ESP |
| EMAIL_ACCEPTED | [INT]_email_accepted    | Receiving message id from ESP |
| EMAIL_BOUNCE | [INT]_email_bounce   | email is bounced back from the user's inbox, reported by the ESP. |
| EMAIL_CLICK | [INT]_email_click | Clicking a URL in email body |
| EMAIL_CONTROL_GROUP | [INT]_email_control_group | Having email assigned to control group |
| EMAIL_DELIVERED | [INT]_email_delivered | Having email delivered to the user |
| EMAIL_QUEUED | [INT]_email_queued |  Having email queued to be sent to the user |
| EMAIL_OPEN  | [INT]_email_open| Email open. |
| EMAIL_REJECTED | [INT]_email_rejected | Email rejection |
| EMAIL_UNSUBSCRIBE | [INT]_email_unsubscribe | Unsubscription on email. |

### SMS System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
|SMS_SENT|[INT]_sms_sent|Sending SMS|
|SMS_ACCEPTED|[INT]_sms_accepted|Receiving message id from SMS server|
|SMS_CLICK|[INT]_sms_click|Clicking on a link in SMS body|
|SMS_CONTROL_GROUP|[INT]_sms_control_group|Having SMS assigned to control group|
|SMS_QUEUED|[INT]_sms_queued|Having SMS queued to be sent to the user|
|SMS_DELIVERED|[INT]_sms_delivered|Delivery of SMS to the user|
|SMS_REJECTED|[INT]_sms_rejected|Rejection of SMS by server|

### WhatsApp System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
|WHATSAPP_SENT|[INT]_whatsapp_sent|Sent from panel|
|WHATSAPP_ACCEPTED|[INT]_whatsapp_accepted|Accepted by provider|
|WHATSAPP_QUEUED|[INT]_whatsapp_queued|Having meassage queued to be sent to the user|
|WHATSAPP_DELIVERED|[INT]_whatsapp_delivered|display it to the user|
|WHATSAPP_CLICK|[INT]_whatsapp_click|Clicking a button in whatsapp message body|
|WHATSAPP_CONTROL_GROUP|[INT]_whatsapp_control_group|When someone falls into the control group|
|WHATSAPP_REJECTED|[INT]_whatsapp_rejected|When rejected by provider|

### Telegram System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
|TELEGRAM_SENT|[INT]_telegram_sent|Sent from panel|
|TELEGRAM_ACCEPTED|[INT]_telegram_accepted|Accepted by provider|
|TELEGRAM_QUEUED|[INT]_telegram_queued|Having meassage queued to be sent to the user|
|TELEGRAM_DELIVERED|[INT]_telegram_delivered|display it to the user|
|TELEGRAM_CLICK|[INT]_telegram_click|Clicking a button in telegram message body|
|TELEGRAM_CONTROL_GROUP|[INT]_telegram_control_group|When someone falls into the control group|
|TELEGRAM_REJECTED|[INT]_telegram_rejected|When rejected by provider|

### Custom Channel System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
|CUSTOM_SENT|[INT]_custom_sent|SMS sent|
|CUSTOM_ACCEPTED|[INT]_custom_accepted|Receiving message id from custom channel provider|
|CUSTOM_CLICK|[INT]_custom_click|Clicking on a link in custom notification body|
|CUSTOM_CONTROL_GROUP|[INT]_custom_control_group|Having custom message assigned to control group|
|CUSTOM_QUEUED|[INT]_custom_queued|Having message queued to be sent to the user|
|CUSTOM_DELIVERED|[INT]_custom_delivered|Delivery of custom notification|
|CUSTOM_REJECTED|[INT]_custom_rejected|Rejection of custom notification|

### Session Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
|BEGIN_SESSION|[INT]_begin_session|Beginning of a user session|
|UPDATE_SESSION|[INT]_update_session|On getting heartbeat from a user session|
|END_SESSION|[INT]_end_session| End of a user session|

### Journey Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
| GOAL_ACCOMPLISH | [INT]_goal_accomplish | When a user performs the Conversion Event defined for a campaign/journey. |
| JOURNEY_END | [INT]_journey_end | Journey end |
| JOURNEY_START | [INT]_journey_start | Journey start |

### Survey System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
| SURVEY_VIEW | [INT]_survey_view |Display survey to user. |
| SURVEY_SUBMIT | [INT]_survey_submit |Close survey by clicking close button |
| SURVEY_CLOSE | [INT]_survey_close |Submit survey by clicking submit button |

### Relay Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
| RELAY_START | [INT]_relay_start | Relay started |
| RELAY_END | [INT]_relay_end | Relay ended |

### User System Events

| Name on Dashboard | Name in Backend  | Description       |
|-------------------|------------------|-------------------|
|LOGIN|[INT]_login|User login|
|LOGOUT | [INT]_logout | User Logged out |

:::note Important
The ability to send/update user attributes using SDKs is disabled by default and sdk only can create an empty user profile if the profile does not already exist.
To enable this feature, please contact our support team and request activation for your project.

⚠️ Note: We always recommend to use our REST API for sending/updating user profile, because it is more flexible and secure way to manipulate user profiles and make the update easier. also Rest API allows your marketing team to have user details before sdk integration.
:::

:::note

 The System Events, User Login, and User Logout are not automatically tracked for your users. You will need to call the respective SDK functions whenever these actions occur on your platforms. These actions are also ideal moments to identify your users.

:::

## Custom Events
Custom Events are behavioral data points that you can custom define and track for your users across your apps and website. These enable you to understand your users better and deliver contextually personalized experiences in real-time.

Depending on your business, these events could be anything like:

* Product Page Viewed
* Susbcription Purchased
* Checkout Started | Checkout Completed
* Review Submitted and so on.

## Event Attributes
Event Attributes are details attached to each Event that convey the context in which a user performed it.

### System Attributes
These are generic details that have been predefined by us and are automatically tracked for all the System Events and your Custom Events. These data points cannot be modified by you.
| Name on Dashboard | Type  | Description       |
|-------------------|------------------|-------------------|
| instance_id	 | String | It can be seen in all events|
|email_id|String|It can be seen just in email system events|
|esp_name|String|It can be seen just in email system events|
|hashed_email|String|It can be seen just in EMAIL_REJECTED and EMAIL_SENT system events|
|esp_message_id	|String|It can be seen just in EMAIL_CLICK, EMAIL_DELIVERED and EMAIL_ACCEPTED system events|
|attempt|Numeric|It can be seen just in EMAIL_BOUNCE system event|
|url|String|It can be seen in all system click events|
|ip|String|It can be seen in EMAIL_CLICK system event|
|reason|String|It can be seen in all QUEUED events|
|new_time_to_live|Numeric|It can be seen in all QUEUED events|
|send_next|Date|It can be seen in all QUEUED and REJECTED events|
|end_reason|String|It can be seen in JOURNEY_END event|
|package_name|String|It can be seen in FCM_REGISTERED and PUSH_NOTIFICATION_CLICK event|
|message_id|String|It can be seen in ACCEPTED events|
|button|Numeric|It can be seen in push click events|
|error_message|String|It can be seen in all REJECTED events|
|ssp_name|String|It can be seen in SMS_SENT, SMS_REJECTED and SMS_ACCEPTED events|
|to_number|String|It can be seen in SMS_REJECTED and SMS_ACCEPTED events |

### Custom Attributes
Custom Attributes are details that can be attached to each [Custom Event](#custom-events) defined by you. You can choose to attach a maximum if 70 custom attributes of a single data type to all Custom Event to better understand each user's platform interactions and contextually engage them.
### Tracking Custom Events & Custom Event Attributes
[System Events](#system-events) and [System Event Attributes](#system-attributes) are automatically tracked by **inTrack** once you integrate a platform through our SDKs. However, you will need to specifically define and pass each [Custom Event](#custom-events) and their [Custom Attributes](#custom-attributes) from your various platforms to your **inTrack** account.

Here are a few guidelines to help you get started:
* Please ensure consistent usage of the names of [Custom Events](#custom-events) and their [Custom Attributes](#custom-attributes) across all your apps (Android, iOS) and website. Doing so will make it easier for you to segment users, personalize campaigns and configure campaign targeting in [your dashboard](https://dash.intrack.ir).
- We highly recommend that you create an excel sheet to list down:

    - The ‍‍‍‍‍[Custom Events](#custom-events) you want to track.
    - The corresponding [Custom Attributes](#custom-attributes) of each Event.
    - The data type of the values you will be tracking against each [Custom Attribute](#custom-attributes).
* The first datapoint synced to **inTrack** defines the data type for that event attribute. Thus, data types must be consistent with the value that you want to store against the attribute. If the data type is changed at a later date, then Custom Event Attribute data will stop flowing to your [inTrack dashboard](https://dash.intrack.ir).
* You can create a maximum of 70 Event Attributes of each data type for a [Custom Event](#custom-events). (ie. 70 attributes of Number data type, 70 attributes of String data type and so on).
* Event name must only contain Alphanumeric characters and hyphens (- _).
* Each key of eventDetails HashMap, must be less than 50 character (otherwise it will be removed by the SDK).
