---
title: Business Events
version: 3.0.0+
source_path: rest-api/business-events.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/rest-api/business-events.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/rest-api/business-events.md
---
# Business Events

**inTrack** offers an API for triggering business events using the business events endpoint as described below.

Any significant action that occurs on the business's end can trigger a business event. Business Events are a comprehensive type of event that covers the entire gamut of actions occurring within the business. This allows you to initiate a series of workflow-based communications in relays based on specific business activities.

**Method:** POST

**URL STRUCTURE:** `<HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/businessEvents`

**AUTHENTICATION:** `Authorization: Bearer <YOUR_API_KEY>`

#### Headers

| Name                                       | Description                                                                                                                                                                                                                                                                                      |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ```x-request-id:<HEX_STRING>```            | Replace `{HEX_STRING}` with a unique random hex string.If API re-attempts are being made, this tag stops propagation of duplicate events within a 4 hour window. Events with same ID re-attempted within 4 hours won't get duplicated if the previous HTTP request was able to ingest the event. |
| ```Authorization: Bearer <YOUR_API_KEY>``` | Replace `<YOUR_$INTRACK_API_KEY>` with your inTrack API KEY.                                                                                                                                                                                     |

### Event Attributes

| Name            | Type                | Description                                                                             |
|-----------------|---------------------|-----------------------------------------------------------------------------------------|
| eventName       | String              | Name of the business event.	.                                                           |
| eventTime       | String              | Date and time when the business event occurred in ISO format: yyyy-MM-ddTHH:mm:ss±hhmm. |
| eventAttributes | `List<String, Any>` | An object which contains name and values of the Business Event Attributes               |

:::warning

Sending `eventName` is mandatory.

:::

#### Guidelines

* Event names must exist in Business Events already defined in [dashboard](https://dash.intrack.ir).

* Event attributes must have been defined for the event.

#### Requests Example

```bash
{
`curl -X POST <HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/businessEvents  \\
    --header 'Authorization: Bearer <YOUR_$INTRACK_API_KEY>' \\
    --header 'Content-Type: application/json' \\
    --data '{
        "eventName": "str",
        "eventTime": "2021-07-09T03:28:38+04:30",
        "eventAttributes": {
            "businessEventAttributeName1": {},
            "businessEventAttributeName2": {},
            "businessEventAttributeName3": {}
        }
    }'`
}
```

#### Response

200 - The request has been successfully accepted.

```json
{
  "result": "queued"
}
```
