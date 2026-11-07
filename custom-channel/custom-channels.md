---
title: Custom Channels
version: 3.0.0+
source_path: custom-channel/custom-channels.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/custom-channel/custom-channels.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/custom-channel/custom-channels.md
---
# Custom Channels

Custom Channel as a Provider is a feature in **inTrack** that allows users to create and customize their own channels for content delivery. This can be useful for businesses or individuals who want to have more control over the content they are distributing.

## Add a new Custom Channel as a Provider

To create a new custom channel, As shown below, navigate to the [inTrackPanel](https://dash.intrack.ir) -> Settings-> Channels-> Custom Channel of your [dashboard](https://dash.intrack.ir) and click on the + icon to add a channel.

![Docusaurus](/img/custom-channel/custom-1.png)

complete required fields

* **Name:** Enter Custom Channel's name

* **Method:** POST, GET, PUT, DELETE
* **URL:** Enter URL of your API channel
* **BODY**: All available variables are set as default. You can delete items that are not needed.

Available Variations:

```json
{
    "Id": "{{id}}",
    "token": "{{token}}",
    "Message": "{{variation.message}}",
    "Subject": "{{variation.subject}}",
    "Image": "{{variation.image}}",
    "keyValuePair": "{{variation.keyValuePairMap}}",
    "Icon": "{{variation.icon}}",
    "Badge": "{{variation.badge}}",
    "attachments": "{{variation.attachmentsArray}}",
    "userId": "{{user.userId}}",
    "anonymousId": "{{user.anonymousId}}"
}
```

* **Headers:** Enter keys and values

:::note

The fields id, token, anonymousId, and userId are automatically set by the system.

:::

## Add a new Custom Channel

Navigate to the [inTrackPanel](https://dash.intrack.ir)-> Engage -> Custom Channel of your [dashboard](https://dash.intrack.ir) and click on the + icon to add a Custom Channel.

![Docusaurus](/img/custom-channel/custom-2.png)

As shown above, after completing Audience and When Steps, on the Message step then select your Custom Channel Provider and complete the required variables that you configured as the body of Custom Channel provider.

## Interaction with custom channels

### Response to API Call

When making an API call to the endpoint defined in the panel, a successful response should return a status code of 200. Any status code other than 200 indicates a failure on the inTrack side. Upon receiving a status code of 200, the message status will change to `Sent`.

###

:::info

You can check custom channels system events [here](../events#custom-channel-system-events)

:::

## Message Status Tracking

### Delivery Status

```
{`curl -X PUT '$https://api.intrack.ir/api/sdk/track/manualDelivery?token={token}&status={status}'`}
```

- **token** (required): Unique trackingId provided to your send endpoint
- **status** (required):
    - `0` → Delivered
    - `1` → Rejected (opt-out)
    - Any other number → Failed

This endpoint should be called to update message delivery status.

### Open Tracking

```
{`curl -X PUT '$https://api.intrack.ir/api/sdk/track/opened?token={token}'`}
```

- **token** (required): Unique trackingId provided to your send endpoint

This endpoint should be called when a message is opened.

### Click Tracking

```
{`curl -X PUT '$https://api.intrack.ir/api/sdk/track/clicked?token={token}&button={button?}&url={url?}'`}
```

- **token** (required): Unique trackingId provided to your send endpoint
- **button** (optional): Identifier of the clicked button
- **url** (optional): Destination URL

This endpoint should be called when a user clicks a link or button in your message.

:::note

If the provided `token` is invalid, the request will be rejected with `400`.
On success, all endpoints return `200`.

:::
