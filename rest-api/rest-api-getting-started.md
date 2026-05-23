---
title: Getting Started
version: 3.0.0+
source_path: rest-api/rest-api-getting-started.md
docs_url: https://docs.intrack.ir/docs/getting-start/rest-api/rest-api-getting-started
---
# Getting Started

REST API allows you to programmatically transfer information over the internet using a predefined schema. **inTrack** offers endpoints with specific requirements that perform actions and return data.

With the **inTrack** APIs you can:

* Track each user's preferences and their actions as User Attributes and Events, respectively.

* Send [Business Events](business-events.md) data to build Relays that enables you to automate product/platform communication. (How it works)

* Personalize a Journey campaign with real-time data from your servers OR update user details as per their journey experience. (How it works)

And much more!

## API Endpoint `<HOST>`

All available resources (/users, /events, /businessEvents, /refresh) can be accessed over HTTPS via the respective URLs listed below. The host URL serves as the parent link of the actual API resource endpoint.

```json
{
  `$https://api.intrack.ir/
`
}
```

| Name                  | Endpoint                                                                                        | Description                                 |
|-----------------------|-------------------------------------------------------------------------------------------------|---------------------------------------------|
| Refresh token         | /api/sdk/accounts/`<YOUR_LICENSE_CODE>`/refresh                                                 | Revoke a token in jwt authorization method. |
| Tracking users        | /api/sdk/accounts/`<YOUR_$INTRACK_LICENSE_CODE>` /users         |                                             |
| Tracking users batch  | /api/sdk/accounts/`<YOUR_$INTRACK_LICENSE_CODE>`/usersBatch     | Sending a list of users in batch request    |
| Tracking events       | /api/sdk/accounts/`<YOUR_$INTRACK_LICENSE_CODE>`/events         |                                             |
| Tracking events batch | /api/sdk/accounts/`<YOUR_$INTRACK_LICENSE_CODE>`/eventsBatch    | Sending a list of events in batch request   |
| Business Events       | /api/sdk/accounts/`<YOUR_$INTRACK_LICENSE_CODE>`/businessEvents |                                             |

:::info

 Check your **LICENSE_CODE**, in [inTrackPanel](https://dash.intrack.ir)->setting->Rest API->LICENSE CODE.

:::

## 1.API Authorization

**inTrack** supports two ways of handling API authorization.

* **1:** Using a pre-defined static key.

* **2:** Refreshable JWT Token.

### Static API Key

A unique REST API key is automatically created for  **inTrack** Account. Your key acts as the authorization bearer token and must be passed in the header of each API call. The token is valid for the entire lifetime of an Account. We strongly recommend you revoke the key periodically.
To access your API key check [inTrackPanel](https://dash.intrack.ir)->setting->Rest API->API KEY.

```
Authorization: Bearer <YOUR_API_KEY>
```

![apiKey](/img/rest/apikey.jpg)

### Refreshable JWT Token

In this method, tokens are generated to access **inTrack** APIs instead of using static keys. These tokens are valid for one hour and can be used to generate a new token within their validity period. Multiple tokens can be generated and used simultaneously, and each token can be revoked separately at any time.

You can view and manage the list of generated tokens in the panel by navigating to [inTrackPanel](https://dash.intrack.ir)->Settings -> Rest API Token.
 Token must be passed in the header of each API call.

```
Authorization: Bearer <YOUR_API_KEY>
```

![apiKey](/img/rest/jwt.jpg)

:::note

To activate JWT, please contact the support team.

:::

#### Refreshing Token

Tokens are valid for one hour. For continuous usage, tokens must be used sequentially to regenerate new tokens. Use the following cURL command to generate a new token using an existing one:

```bash
{
  `curl -X POST "<HOST>/api/sdk/accounts/<YOUR_LICENSE_CODE>/refresh"  \\
      --header "Content-Type: application/json"  \\
      --data '{
        "refresh_token": "<TOKEN>"
      }'
`
}
```

#### Revoking Token

To revoke a token, use the following cURL command:

```bash
{
  `curl -X POST "<HOST>/api/sdk/accounts/<YOUR_LICENSE_CODE>/revoke"  \\
      --header "Content-Type: application/json" \\
      --data '{
        "token": "<TOKEN>",
        "identifier": "<IDENTIFIER>"
      }'
`
}
```

:::note

Revoking a token will invalidate all implementations using the revoked token.

:::

:::note

When generating tokens, ensure you note down the access token and refresh token as they will not be displayed again after you leave the page.

:::
