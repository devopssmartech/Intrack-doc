---
title: Transactional Campaign API
version: 3.0.0+
source_path: rest-api/transactional-campaign-api.md
docs_url: https://docs.intrack.ir/docs/getting-start/rest-api/transactional-campaign-api
---
# Transactional Campaign API

## Step-by-Step Guide to Setting Up the inTrack Transactional Campaign API

The **inTrack** Transactional Campaign API enables you to send critical transactional messages to your users, such as order confirmations, shipping details, payment invoices, and more through channels like Push, SMS, Email, Web Push, and Custom Channel.

**Important Note:** This API can only be used to trigger transactional campaigns that are in the "Running" state. The campaign must be launched on the [inTrack dashboard](https://dash.intrack.ir) before calling this API.

### API Endpoint

**Method:** POST

**URL Structure:**

```
<HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/transactionCommunication
```

**Header Structure:**

Authorization: Bearer `<YOUR_$INTRACK_API_KEY>`
x-request-id: `<HEX_STRING>`

#### Request Headers

| Name          | Description                                                                                                                                                                   |
|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Authorization | Bearer `<YOUR_$INTRACK_API_KEY>` Replace `<YOUR_$INTRACK_API_KEY>` with your inTrack API Key. |
| x-request-id  | `<HEX_STRING>` Replace `<HEX_STRING>` with a unique random hex string. Prevents duplicate events within 4 hours.                                                              |

### Example API Request

```bash
{
`curl -X POST <HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/transactionCommunication \\
    --header 'Authorization: Bearer <YOUR_$INTRACK_API_KEY>' \\
    --header 'Content-Type: application/json' \\
    --data '{
      "communicationId": 5,
      "userId": "userId",
      "anonymousId": null,
      "variationTokens": {
        "someMessageToken": "desired value"
      },
      "overrideData": {
        "email": "12@3",
        "phone": "09379897845",
        "queueMinutes": 2
      }
    }'`
}
```

### Guidelines

| Parameter                 | Type                 | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Mandatory                    |
|---------------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------|
| userId                    | String               | Identifier for known user. Either one of userId or anonymousId is mandatory. User ID or anonymous ID can be found on the respective user’s profile screen on [inTrack dashboard](https://dash.intrack.ir). **Constraint**: - userId can be of maximum 100 characters.                                                                                                                                                                                                              | Either userId or anonymousId |
| anonymousId               | String               | Identifier for anonymous user. Unknown User ID can be found on the respective unknown user’s profile screen on [inTrack dashboard](https://dash.intrack.ir). **Constraint**:  - anonymousId can be of maximum 100 characters.                                                                                                                                                                                                                                                      | Either userId or anonymousId |
| communicationId           | integer              | The ID of the campaign which you would like to trigger via this API. **Constraint**:- The communication type of campaign should be transactional.                                                                                                                                                                                                                                                                                                                                                           | Yes                          |
| overrideData.queueMinutes | integer              | This parameter specifies the maximum time that **inTrack** should take to send your message to the user once the message has been received by **inTrack**. Delays can happen sometimes because of infrastructure issues or other errors. In case of any delays, the request will be retried multiple times (in order to send the message) only for the time duration specified in the queueMinutes. We use a default queueMinutes of 60 seconds in case it is not provided. | No                           |
| overrideData.email        | String               | Specify the email ID to whom the email should be sent. Default value for this property is the user's email. If the property was not defined in user attributes, for Email channel the email won't send.                                                                                                                                                                                                                                                                                                     | No                           |
| overrideData.phone        | String               | Specify the phone number to whom the SMS should be sent. Default value for this property is the user's phone. If the property was not defined in user attributes, for SMS channel the sms won't send.                                                                                                                                                                                                                                                                                                       | No                           |
| variationTokens           | `List <String, Any>` | An object which contains values of personalized tokens which are used in the campaign message.  **Eg.** If the campaign's message contains the following token: `{token["X"]}}` then the variationTokens should be: `"variationTokens":{"X": "value"}`                                                                                                                                                                                                                                                      | No                           |

**A transactional campaign with the communicationId should exist.**

**The user(known/unknown) should already define otherwise the api call will do nothing.**

### Response

200 - The request has been successfully accepted.

```json
{
  "result": "queued"
}
```
