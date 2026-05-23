---
title: Private Providers
version: 3.0.0+
source_path: custom-channel/private-providers.md
docs_url: https://docs.intrack.ir/docs/getting-start/custom-channel/private-providers
---
# Private Providers

Many businesses are hesitant to share their users' contact details with third-party platforms like inTrack. We understand this concern, so we’ve developed Private Providers (SSP & ESP) to address it.

Private Providers act as a proxy layer that decrypts the contact information of a campaign's target audience before sending it to your actual provider for delivery. Here’s how you can implement this:

* **Pass Hashed Contact Information:** Provide hashed contact details (hashedPhone and hashedEmail) of your users to **inTrack**.

* **Set Up Private ESP/SSP API Endpoint:** Create an API endpoint that decrypts the hashed addresses and forwards them to your actual provider for delivery. Inform **inTrack** of this setup through our [dashboard](https://dash.intrack.ir) by configuring a new channel.
* **Relay Delivery Status:** Ensure that the delivery status of each message is relayed back from your provider to **inTrack**.

* **Select Private Provider:** When creating campaigns, select your Private Provider as the preferred provider. **inTrack** will then send all messages to the specified endpoint, where you can decrypt user data and pass it to your preferred provider.

By following these steps, you can maintain the privacy of your users' contact information while leveraging **inTrack**'s capabilities.

## Passing Hashed data

All **inTrack** platform integration SDKs allow you to pass hashed phone numbers and email addresses for each user. To utilize this feature, include the hashedPhone and hashedEmail properties in the userDetails. For example:

```bash
{
`curl -X POST <HOST>/api/sdk/accounts/<YOUR_$InTrack_LICENSE_CODE>/users \\
    --header 'Authorization: Bearer <YOUR_$InTrack_API_KEY>' \\
    --header 'Content-Type: application/json' \\
    --data '{
        "userId" : "str", // for your known users
        "anonymousId" : "str", // for your unknown users
        "hashedPhone" : "+98********67",
        "hashedEmail" : "u1******.com",
    }'`
}
```

## API Endpoint for Private ESP/SSP

A Private ESP/SSP is an API endpoint that you expose for **inTrack** to call, serving as a proxy between **inTrack** and your actual service provider.

* **Request Handling:** **inTrack** sends a payload to your Private ESP/SSP endpoint containing the hashed identifiers, the message body, and additional data.

* **Response Expectation:** **inTrack** expects an immediate JSON response indicating the success or failure of the request.

* **Delivery Status Notifications:** **inTrack** also provides a delivery endpoint that expects later hits to pass subsequent Delivery Status Notifications (e.g., delivery status such as sent or bounced).

## Automatic Delivery

If your provider is integrated with **inTrack**, we can automatically retrieve the delivery status of messages. This means you don't need to manually send delivery status updates to **inTrack**. For more details about the list of our current providers, refer to the Channel section in your [dashboard](https://dash.intrack.ir).

In the following sections, we will discuss the details of each API request and response model.

## Private Email Service Provider APIs

**inTrack** makes POST requests to an API endpoint URL that you provide, with key-value pairs in the headers as configured in your [dashboard](https://dash.intrack.ir).

### Request

Here is a sample request:

```bash
{
`curl -X POST <YOUR_DESIRED_URL> \\
    --header 'Content-Type: application/json' \\
    --header 'YOUR_KEY: YOUR_VALUE' \\
    --data-raw '{
        "html" : "string",
        "subject" : "string",
        "fromEmail" : "string",
        "fromName" : "string",
        "to": [{
            "email" : "string",
            "name" : "string",
            "type" : "string" // "TO" | "CC" | "BCC"
        }],
        "replyTo" : "string",
        "attachments" : [{
            "type" : "string", // media type
            "name" : "string",
            "content" : "string" // base64
        }],
        "trackingId": "string" // will be null in automatic delivery
    }'`
}
```

:::note

In the case of automatic tracking, trackingIds will not be sent.

:::

### Required Response

In response to the `$inTrack request`, you should provide appropriate data as shown below. If the response is not provided correctly, inTrack will record an Email rejected event.

```json
{
    "data": [
        {
            "messageId": "string",
            "email": "string",
            "status": 0
            // 0: sent - messageId is valid
            // 1: rejected - email was blocked or invalid, or any state that results in opting user out
            // else: failed - message not sent, status will be logged
        }
    ]
}
```

#### Key Descriptions:

| Key       | Type   | Description                              |
|-----------|--------|------------------------------------------|
| messageId | String | The identifier provided by the provider. |
| email     | String | Same value that you received in the request. |
| status    | Numeric| Delivery status of the message.          |

#### Status Codes:

* **0:** sent - messageId is valid

* **1:** rejected - email was blocked or invalid, or any state that results in opting the user out

* **Any other value:** failed - message not sent, status will be logged

### Automatic Email Delivery

To enable automatic delivery:

* **1: Service Provider Compatibility:** Ensure that the email service you are using is one of the service providers supported by inTrack.

* **2:Configure Delivery SSP:**

Navigate to the [inTrack dashboard](https://dash.intrack.ir).
Go to Channels -> SSP -> Action -> Edit -> Delivery SSP.
Set the "Delivery SSP" accordingly.

* **3:Tracking ID:** Ensure that the trackingId included in your response matches exactly with the ID provided by your email service provider.

By following these steps, inTrack will handle automatic delivery status updates for your emails.

### Manual Email Delivery Endpoint

If automatic tracking is not possible, you can manually send the delivery status of SMS messages by calling the following endpoint:

```
{`curl -X PUT '$https://api.intrack.ir/api/sdk/track/manualDelivery?token={token}&status={status}'`}
```

- **token** (required): Unique trackingId provided to your send endpoint
- **status** (required):
    - `0` → Delivered
    - `1` → Rejected (opt-out)
    - Any other number → Failed

This endpoint should be called to update message delivery status.

:::note

If the provided `token` is invalid, the request will be rejected with `400`.
On success, all endpoints return `200`.

:::

## Private SMS Provider APIs

**inTrack** sends POST requests to an API endpoint URL provided by you, including key-value pairs in headers configured through your [dashboard](https://dash.intrack.ir).

### Request

Below is a sample request format:

```bash
curl -X POST '<YOUR_DESIRED_URL>' \
    --header 'YOUR_KEY: YOUR_VALUE' \
    --header 'Content-Type: application/json' \
    --data-raw '{
        "recipients": ["string"], // List of recipient phone numbers
        "messages": ["string"], // List of messages corresponding to recipients
        "trackingIds": ["string"] // Null for automatic delivery, used for manual delivery
    }'
```

### Required Response

Upon receiving a request from **inTrack**, your API should respond with appropriate data as shown below. Failure to provide a valid response will result in **inTrack** recording an SMS rejected event.

```json
{
    "responses": [
        {
            "messageId": "string",
            "status": 0
            // 0: Sent - messageId is valid
            // 1: Rejected - Message was blocked or invalid, or recipient opted out
            // Any other value: Failed - Message not sent, status will be logged
        }
    ]
}
```

#### Response Details:

**messageId:** String identifier provided by your SMS provider.

**status:** Numeric code indicating the delivery status of the message.

#### Status Codes:

* **0:** sent - messageId is valid
* **1:** rejected - email was blocked or invalid, or any state that results in opting the user out
* **Any other value:** failed - message not sent, status will be logged

### Automatic SMS Delivery

To enable automatic SMS delivery through inTrack, follow these steps:

**1:** Ensure that your SMS service provider is integrated with inTrack.

**2:** Set the "Delivery SSP" in the [inTrack dashboard](https://dash.intrack.ir) by navigating to:
[inTrack dashboard](https://dash.intrack.ir) -> Channels -> SSP -> Action -> Edit -> Delivery SSP.

**3:** When responding to inTrack with delivery status updates, ensure that the trackingId included matches the exact identifier provided by your SMS service provider.

By completing these steps, inTrack can seamlessly manage and track the delivery of SMS messages through your integrated service provider.

### Manual SMS Delivery Endpoint

If automatic tracking is not possible, you can manually send the delivery status of SMS messages by calling the following endpoint:

```
{`curl -X PUT '$https://api.intrack.ir/api/sdk/track/manualDelivery?token={token}&status={status}'`}
```

- **token** (required): Unique trackingId provided to your send endpoint
- **status** (required):
    - `0` → Delivered
    - `1` → Rejected (opt-out)
    - Any other number → Failed

This endpoint should be called to update message delivery status.

:::note

If the provided `token` is invalid, the request will be rejected with `400`.
On success, all endpoints return `200`.

:::
