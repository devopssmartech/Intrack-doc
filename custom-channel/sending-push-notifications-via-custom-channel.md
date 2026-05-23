---
title: Sending Push Notifications via Custom Channel
version: 3.0.0+
source_path: custom-channel/sending-push-notifications-via-custom-channel.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/custom-channel/sending-push-notifications-via-custom-channel.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/custom-channel/sending-push-notifications-via-custom-channel.md
---
# Sending Push Notifications via Custom Channel

## Overview of Sending Push Notifications via Custom Channels

The process of sending push notifications through **inTrack**'s custom channels involves the following steps. Depending on your needs, some steps may require additional development on your end:

* **Message Delivery (Required):**
This step is mandatory and involves setting up a service to receive campaign data from inTrack and forward it to a push provider, such as FCM (Firebase Cloud Messaging). You will need to develop this integration to ensure campaign messages are delivered to users.

* **Message Display and Event Tracking (Optional):**
This step is optional. You can use **inTrack**'s SDK to handle the display of notifications and manage related event tracking. Alternatively, if preferred, you may implement custom handling within the client-side application to manage notification display and event tracking.

* **User Reachability (Optional):**
Making users reachable is also an optional step. However, implementing this step can enhance the targeting and effectiveness of your campaigns by allowing you to better track user engagement and accessibility.

### Message Delivery (Developing a Backend Middleware for Delivery)

In this step, it's essential to develop an API as a middleware between **inTrack** and the push notification provider (e.g., FCM) on the client's backend.The details of this API should be [provided to inTrack as a custom channel through the user panel](#1add-new-custom-channel-as-a-provider).

For example, let's assume you develop an API endpoint at `https://your-domain.com/pushProxy`. This API will:

Receive custom campaign data from inTrack,
Convert the data into a push notification payload format,
Forward it to FCM for delivery to end users.
Details on how to structure and convert the data into the appropriate payload format for FCM will be explained in the following sections. This setup enables inTrack to interact seamlessly with your push provider, allowing for efficient and tailored message delivery.

#### 1.Add new custom channel as a provider

To create a new custom channel, As shown below, navigate to the [inTrackPanel](https://dash.intrack.ir) -> Settings-> Channels-> Custom Channel of your [dashboard](https://dash.intrack.ir) and click on the + icon to add a channel.

![Docusaurus](/img/custom-channel/custom-1.png)

complete required fields

* **Name:** Enter Custom Channel's name

* **Method:** POST, PUT
* **URL:** Enter URL of your API channel
* **BODY**: All available variables are set as default. You can delete items that are not needed.

***If needed, specify the required headers for your API (for example, the header related to Authorization).***

When sending a custom type campaign to this new channel, the campaign data will be sent to your API in JSON format (the body parameters as shown in the image above).

#### 2.Constructing and Sending the Push Notification

In this stage, after identifying the user and obtaining their corresponding push token, you will need to convert this data into the required message format for your chosen push provider before sending it.

For example, when communicating with FCM, inTrack sends a message formatted as follows:

```json
{
  `{ "to": "<CUSTOMER_PUSH_TOKEN>",
  "data": {
    "id": "<CUSTOM_CHANNEL_TOKEN>",
    "title": "PUSH TITLE",
    "message": "PUSH BODY",
    "media": "URL OF PUSH IMAGE",
    "source": "$inTrack",
    "link": "DEEPLINK (click on push body)",
    "buttons": [
      {
        "t": "TITLE OF BUTTON 1",
        "l": "DEEPLINK"
      },
      {
        "l": "DEEPLINK"
      }
    ],
    "customData": {
       "KEY": "VALUE"
      }
    },
     "priority": "high"
  }
`
}
```

:::note

If you intend to display notifications on the user's device yourself, you can send the message according to your own structure. However, if you want to use the **inTrack** SDK to display the message and send related events, it is essential to adhere to the structure outlined above.

:::

* Note that in any case, you must send the token received from inTrack to the client. This token is used to track delivery and click events. Further details will be provided in the next step

### Displaying Messages and Sending Events (Client-Side)

In this stage, three main actions are performed: **displaying the message**, **sending the delivery event to inTrack**, and **sending the click event (if it occurs) to inTrack**. If you delegate the message display to the SDK, all these processes will be managed by the SDK. However, please note that in order for the **inTrack** SDK to process the message correctly, you must send the message according to the structure defined in the [previous section](#2constructing-and-sending-the-push-notification).

If you plan to display the message yourself, in addition to developing the display method on the client side, you must also send the delivery and click events to inTrack.

* **Using REST API:** In this case, simply call the necessary APIs for these events as documented. You can find the relevant documentation  here: for [Registering delivery of message](#registering-delivery-of-message) and for [click event](#registering-click-action).

* **Using SDK:** Since the process of receiving push notifications typically occurs when your application is in the background or closed, you may not be able to invoke the click and delivery APIs promptly. In this situation, we recommend utilizing the relevant methods in the SDK. These methods will store events if they cannot be registered immediately and will send them once the app is reopened. You can view the documentation for sending events via the SDK here: [inTrack Android SDK Documentation](../../getting-start/android/tracking-events#sending-custom-channel-events).

##### Registering delivery of message
When a message is successfully delivered to its intended recipient, it can be registered with **inTrack** by sending a `GET` request to the endpoint `/api/sdk/track/delivered`. This request should include one parameter called **token**, which is a **base64-encoded** string representing a JSON object containing the following values:

**id:** UUID – The unique identifier for the message.
**productId:** Integer – The identifier for the associated product.
**userId:** String – The identifier for the user receiving the message.
**anonymousId:** String – The anonymous identifier, if applicable.

```json
{
  `curl -X GET $https://api.intrack.ir//api/sdk/track/delivered?token=ewogICAgImlkIjogIjEyM2U0NTY3LWU4OWItMTJkMy1hNDU2LTQyNjY1MjM0MDAwMCIsCiAgICAicHJvZHVjdElkIjogMSwKICAgICJ1c2VySWQiOiAiMDAwMSIKfQ
`
}
```

##### Registering click action

When a user clicks on a link or button within a message, this interaction can be registered with the **inTrack** server by sending a `GET` request to the endpoint `/api/sdk/track/clicked`.

This request should include the following parameters:

**token:** This is the required parameter, as described in the previous section.

**button (optional):** An Integer representing the index of the button that was clicked. This parameter helps identify which button in a message was interacted with.

**url (optional):** A String specifying the URL that was clicked. This parameter allows for tracking specific links within the message.

```json
{
  `curl -X GET $https://api.intrack.ir/api/sdk/track/clicked?token=ewogICAgImlkIjogIjEyM2U0NTY3LWU4OWItMTJkMy1hNDU2LTQyNjY1MjM0MDAwMCIsCiAgICAicHJvZHVjdElkIjogMSwKICAgICJ1c2VySWQiOiAiMDAwMSIKfQ&button=1&url=https://example.com
`
}
```

### Marking a User as Reachable

In the process of sending push notifications via a custom channel, marking a user as "reachable" is optional since you handle the storage of the push token and its mapping to the user. However, for more targeted campaigns, it's recommended to send a custom value for each user who has received a push token. This allows **inTrack** to identify the user as reachable and can assist in segmenting users effectively.
To do this, simply call the `onTokenRefresh` method in the SDK with a custom value whenever your app receives a new push token. This enables **inTrack** to recognize the user's availability for targeted notifications and campaign segmentation.
For more details on `onTokenRefresh`, please refer to the: [Android Doc](../../getting-start/android/push-messaging/#3initializing-notification-service)
