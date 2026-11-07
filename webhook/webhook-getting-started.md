---
title: How It Works
version: 3.0.0+
source_path: webhook/webhook-getting-started.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/webhook/webhook-getting-started.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/webhook/webhook-getting-started.md
---
# How It Works

**inTrack** webhooks enable you to set up HTTP callbacks for events occurring within **inTrack**. By configuring a webhook, **inTrack** can trigger a callback to a script on your web server whenever specific events occur, allowing you to receive real-time data for internal use.

Webhooks can be utilized to:

* Send information to external systems.

* Integrate with backend systems for automated processes.

For instance, you might automate actions such as crediting customers' accounts with promotional credits once they've performed a custom event a specified number of times.

Use **inTrack** webhooks to streamline data flow and enhance automation between **inTrack** and your infrastructure.

## Webhook Requests

Webhook requests will be posted to a URL configured by you and in the format described below. Webhook request method will always be `POST`. There will be additional parameters that will be appended to the `postURL`.

### Request Headers

| Name s | Description  |
|-----------------------|--------------------------------------------------------------------------|
| x-request-id:`{HEX_STRING}` | Used to identify webhook POST requests uniquely to achieve events idempotency. `{HEX_STRING}` will be replaced by a unique, random hex string. |

### Request Parameters

| Name         | Description |
|--------------|------------------------------------------------------|
| eventType    | Event for which data is being sent                                                                   |
| licenseCode  | Your **inTrack** license code                                                                             |
| secret       | This secret key is used to identify webhook POST requests from **inTrack**. More details in Request Verification section below. |

### Request Body

The Request body will contain event-specific data and will vary based on the registered webhook.

### Response

**inTrack** will understand HTTP Status, of the response. Responses with HTTP Status 200 will be treated as successful posts, everything else will be logged as failed webhook posts. See the section about error handling and retries below to understand what happens in case of failures.

### Request Verification

You will find `Webhook Key` for your account in the Webhooks menu of [inTrack dashboard](https://dash.intrack.ir)-> webhook-> Webhook Credentials. This key is to be used to identify the Webhook `POST` requests from
**inTrack** to your servers. **inTrack** appends a parameter named secret in the postURL. Value of this parameter is the MD5 Hex of the combination of your **inTrack** license code and the Webhook Key separated by `:`.
