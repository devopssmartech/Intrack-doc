---
title: Telegram Channel
version: 3.0.0+
source_path: custom-channel/telegram-provider.md
docs_url: https://docs.intrack.ir/docs/getting-start/custom-channel/telegram-provider
---
# Telegram Channel

## Create a Telegram Bot

Obtain your Telegram API token by creating a bot through `@BotFather`. Follow [this tutorial](https://core.telegram.org/bots/tutorial) for detailed steps.

Here’s an example of what the token looks like:

```jsx
{`4839574812:AAFD39kkdpWt3ywyRZergyOLMaJhac60qc`}
```

## Start the Telegram Bot

Run the sample **inTrack** Telegram bot by following the instructions provided in the bot’s `README.md` file using **inTrack** [REST API](../rest-api/rest-api-getting-started) credentials.
You can also customize the bot to meet your specific requirements.

To access the bot's source code, please contact us for more details.

## Integrate With inTrack

If you prefer, you can integrate your custom bot with **inTrack** without relying on the sample **inTrack** Telegram bot using [REST API](../rest-api/rest-api-getting-started).
To achieve this, send the `Chat ID` and `Phone Number` of the user to **inTrack** to make them reachable on Telegram.

Here’s a sample CURL request:

```bash
{
`curl -X POST <HOST>/api/sdk/accounts/<YOUR_$INTRACK_LICENSE_CODE>/telegram-info \\
    --header 'Authorization: Bearer <YOUR_$INTRACK_API_KEY>' \\
    --header 'Content-Type: application/json' \\
    --data '{
        "phoneNumber": "980000000000",
        "telegramChatId": "0000000000",
        "optIn": true
    }'
`}
```

:::info

You can checked telegram system events [here](../events#telegram-system-events)

:::
