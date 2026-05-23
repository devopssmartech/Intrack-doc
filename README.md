# inTrack Documentation

**inTrack** is an all-in-one marketing automation platform that helps businesses connect with customers through automated, personalized messaging across mobile apps, web, and APIs.

Track users and events, run push and in-app campaigns, and manage journeys from the [inTrack dashboard](https://dash.intrack.ir).

This repository contains **plain Markdown** integration guides (version **3.0.0+**) exported from the official [inTrack documentation](https://docs.intrack.ir). Open any page below to read the full guide—similar to browsing SDK docs on GitHub.

## Table of contents

### [Overview](#overview)
* [Events](events.md)
* [Getting Started](getting-start.md)
* [Using a Custom Domain](sms-custom-domain.md)
* [User](user.md)

### [Android SDK](#android)
* [Getting Started](android/android-getting-started.md)
* [In-app Messaging](android/inapp-messaging.md)
* [Notification Channels](android/push-messaging/notification-channels.md)
* [Push Amplification](android/push-messaging/push-amplification.md)
* [Push Messaging](android/push-messaging/push-messaging.md)
* [Push notification advance features](android/push-messaging/push-notification-advance-features.md)
* [Tracking Events](android/tracking-events.md)
* [Tracking Users](android/tracking-users.md)

### [iOS SDK](#ios)
* [Getting Started](iOS/ios-getting-started.md)
* [In-app Messaging](iOS/ios-inapp-messaging.md)
* [Push Messaging](iOS/push-messaging.md)
* [Tracking Events](iOS/tracking-events.md)
* [Tracking Users](iOS/tracking-users.md)

### [React Native SDK](#react-native)
* [Push Messaging](react-native/push-messaging.md)
* [Getting Started](react-native/react-native-getting-started.md)
* [In-app Messaging](react-native/react-native-inapp-messaging.md)
* [Tracking Events](react-native/tracking-events.md)
* [Tracking Users](react-native/tracking-users.md)

### [Flutter SDK](#flutter)
* [Getting Started](flutter/flutter-getting-started.md)
* [In-app Messaging](flutter/inapp-messaging.md)
* [Push Messaging](flutter/push-messaging.md)
* [Tracking Events](flutter/tracking-events.md)
* [Tracking Users](flutter/tracking-users.md)

### [Web SDK](#web)
* [On-Site](web/on-site.md)
* [Tracking Events](web/tracking-events.md)
* [Tracking Users](web/tracking-users.md)
* [Getting Started](web/web-getting-started.md)
* [Web Push Messaging](web/web-push-notification.md)

### [REST API](#rest-api)
* [Business Events](rest-api/business-events.md)
* [Getting Started](rest-api/rest-api-getting-started.md)
* [Tracking Events](rest-api/tracking-events.md)
* [Tracking Users](rest-api/tracking-users.md)
* [Transactional Campaign API](rest-api/transactional-campaign-api.md)

### [Custom Channels](#custom-channel)
* [Custom Channels](custom-channel/custom-channels.md)
* [Private Providers](custom-channel/private-providers.md)
* [Sending Push Notifications via Custom Channel](custom-channel/sending-push-notifications-via-custom-channel.md)
* [Telegram Channel](custom-channel/telegram-provider.md)

### [Webhook](#webhook)
* [How to Configure](webhook/configuration.md)
* [How It Works](webhook/webhook-getting-started.md)

### [WebView integration](#web-view)
* [Integration Guide](web-view/implementaion-guide.md)

### [Migration](#migration)
* [Migrating from SDK V2 to V3](migration/migration-v2-to-v3.md)

---

## <a id="overview"></a>Overview

### [Events](events.md)

All behavioral data points are called Events in [your dashboard](https://dash.intrack.ir). And each Event can further be understood in the context of its Attrib

Read the full guide: **[events.md](events.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/events)

### [Getting Started](getting-start.md)

This documentation covers the steps to integrating your apps and website with inTrack. We’ll also discuss a few fundamentals of how the [dashboard](https://dash

Read the full guide: **[getting-start.md](getting-start.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/)

### [Using a Custom Domain](sms-custom-domain.md)

In digital marketing, short and visually appealing SMS links are essential. Intrack automatically shortens any link included in your SMS campaigns, reducing mes

Read the full guide: **[sms-custom-domain.md](sms-custom-domain.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/sms-custom-domain)

### [User](user.md)

At inTrack, anybody who has interacted with your business at least once is called a User.

Read the full guide: **[user.md](user.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/user)

## <a id="android"></a>Android SDK

### [Getting Started](android/android-getting-started.md)

inTrack Android SDK is hosted on mavenCentral Maven repository.

Read the full guide: **[android/android-getting-started.md](android/android-getting-started.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/android/android-getting-started)

### [In-app Messaging](android/inapp-messaging.md)

you can enable In App messaging service by calling `enableInAppMessaging()` on InTrackConfig object during inTrack initialization.

Read the full guide: **[android/inapp-messaging.md](android/inapp-messaging.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/android/inapp-messaging)

### [Notification Channels](android/push-messaging/notification-channels.md)

From Android 8.0 (API level 26) and higher, [notification channels](https://developer.android.com/develop/ui/views/notifications#ManageChannels) are supported a

Read the full guide: **[android/push-messaging/notification-channels.md](android/push-messaging/notification-channels.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/android/push-messaging/notification-channels)

### [Push Amplification](android/push-messaging/push-amplification.md)

Push amplification refers to the process of increasing the effectiveness and reach of push notifications by optimizing delivery strategies, targeting relevant a

Read the full guide: **[android/push-messaging/push-amplification.md](android/push-messaging/push-amplification.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/android/push-messaging/push-amplification)

### [Push Messaging](android/push-messaging/push-messaging.md)

Please ensure that you have completed all the platform integration steps listed under [GettingStarted](../android-getting-started) before proceeding.

Read the full guide: **[android/push-messaging/push-messaging.md](android/push-messaging/push-messaging.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/android/push-messaging/push-messaging)

### [Push notification advance features](android/push-messaging/push-notification-advance-features.md)

The inTrack SDK provides a helper class named InTrackPush designed to offer enhanced control and flexibility in handling push notifications. Below are some comm

Read the full guide: **[android/push-messaging/push-notification-advance-features.md](android/push-messaging/push-notification-advance-features.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/android/push-messaging/push-notification-advance-features)

### [Tracking Events](android/tracking-events.md)

We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their att

Read the full guide: **[android/tracking-events.md](android/tracking-events.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/android/tracking-events)

### [Tracking Users](android/tracking-users.md)

We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will h

Read the full guide: **[android/tracking-users.md](android/tracking-users.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/android/tracking-users)

## <a id="ios"></a>iOS SDK

### [Getting Started](iOS/ios-getting-started.md)

You can add the  inTrack SDK to your project in two ways: using CocoaPods or manually. Before you begin, ensure everything is properly configured:

Read the full guide: **[iOS/ios-getting-started.md](iOS/ios-getting-started.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/iOS/ios-getting-started)

### [In-app Messaging](iOS/ios-inapp-messaging.md)

you can enable In App messaging service by calling `enableInAppMessaging` on InTrackConfig object during inTrack initialization.

Read the full guide: **[iOS/ios-inapp-messaging.md](iOS/ios-inapp-messaging.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/iOS/ios-inapp-messaging)

### [Push Messaging](iOS/push-messaging.md)

Please ensure that you have completed all the platform integration steps listed under [GettingStarted](ios-getting-started) before proceeding.

Read the full guide: **[iOS/push-messaging.md](iOS/push-messaging.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/iOS/push-messaging)

### [Tracking Events](iOS/tracking-events.md)

We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their att

Read the full guide: **[iOS/tracking-events.md](iOS/tracking-events.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/iOS/tracking-events)

### [Tracking Users](iOS/tracking-users.md)

We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will h

Read the full guide: **[iOS/tracking-users.md](iOS/tracking-users.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/iOS/tracking-users)

## <a id="react-native"></a>React Native SDK

### [Push Messaging](react-native/push-messaging.md)

Before continuing, please ensure that you have added [react-native-inTrack](react-native-getting-started.md) to your App.

Read the full guide: **[react-native/push-messaging.md](react-native/push-messaging.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/react-native/push-messaging)

### [Getting Started](react-native/react-native-getting-started.md)

Here's how you can integrate the inTrack SDK with your react native apps:

Read the full guide: **[react-native/react-native-getting-started.md](react-native/react-native-getting-started.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/react-native/react-native-getting-started)

### [In-app Messaging](react-native/react-native-inapp-messaging.md)

you can enable In App messaging service by calling enableInAppMessaging on config object during inTrack initialization.

Read the full guide: **[react-native/react-native-inapp-messaging.md](react-native/react-native-inapp-messaging.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/react-native/react-native-inapp-messaging)

### [Tracking Events](react-native/tracking-events.md)

We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their att

Read the full guide: **[react-native/tracking-events.md](react-native/tracking-events.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/react-native/tracking-events)

### [Tracking Users](react-native/tracking-users.md)

We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will h

Read the full guide: **[react-native/tracking-users.md](react-native/tracking-users.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/react-native/tracking-users)

## <a id="flutter"></a>Flutter SDK

### [Getting Started](flutter/flutter-getting-started.md)

Flutter is an open-source UI software development kit created by Google. It enabled you to develop apps for Android, iOS, Linux, Mac, Windows, and the web from 

Read the full guide: **[flutter/flutter-getting-started.md](flutter/flutter-getting-started.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/flutter/flutter-getting-started)

### [In-app Messaging](flutter/inapp-messaging.md)

you can enable In App messaging service by calling enableInAppMessaging on config object during inTrack initialization.

Read the full guide: **[flutter/inapp-messaging.md](flutter/inapp-messaging.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/flutter/inapp-messaging)

### [Push Messaging](flutter/push-messaging.md)

Before continuing, please ensure that you've [ added the Flutter SDK to your app ](../flutter/flutter-getting-started.md).

Read the full guide: **[flutter/push-messaging.md](flutter/push-messaging.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/flutter/push-messaging)

### [Tracking Events](flutter/tracking-events.md)

We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their att

Read the full guide: **[flutter/tracking-events.md](flutter/tracking-events.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/flutter/tracking-events)

### [Tracking Users](flutter/tracking-users.md)

We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will h

Read the full guide: **[flutter/tracking-users.md](flutter/tracking-users.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/flutter/tracking-users)

## <a id="web"></a>Web SDK

### [On-Site](web/on-site.md)

Initializing On Site messages service is done by calling initOnSiteMessaging method with optional config parameter.

Read the full guide: **[web/on-site.md](web/on-site.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/web/on-site)

### [Tracking Events](web/tracking-events.md)

We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their att

Read the full guide: **[web/tracking-events.md](web/tracking-events.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/web/tracking-events)

### [Tracking Users](web/tracking-users.md)

We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will h

Read the full guide: **[web/tracking-users.md](web/tracking-users.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/web/tracking-users)

### [Getting Started](web/web-getting-started.md)

The inTrack Web SDK enables seamless web push notifications and comprehensive data gathering from web applications.

Read the full guide: **[web/web-getting-started.md](web/web-getting-started.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/web/web-getting-started)

### [Web Push Messaging](web/web-push-notification.md)

Please ensure that you have completed all the platform integration steps listed under [GettingStarted](web-getting-started) before proceeding.

Read the full guide: **[web/web-push-notification.md](web/web-push-notification.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/web/web-push-notification)

## <a id="rest-api"></a>REST API

### [Business Events](rest-api/business-events.md)

inTrack offers an API for triggering business events using the business events endpoint as described below.

Read the full guide: **[rest-api/business-events.md](rest-api/business-events.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/rest-api/business-events)

### [Getting Started](rest-api/rest-api-getting-started.md)

REST API allows you to programmatically transfer information over the internet using a predefined schema. inTrack offers endpoints with specific requirements th

Read the full guide: **[rest-api/rest-api-getting-started.md](rest-api/rest-api-getting-started.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/rest-api/rest-api-getting-started)

### [Tracking Events](rest-api/tracking-events.md)

We recommend that you get yourself acquainted with the concept of [SystemEvents](../events#system-events), [CustomEvents](../events#custom-events) and their att

Read the full guide: **[rest-api/tracking-events.md](rest-api/tracking-events.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/rest-api/tracking-events)

### [Tracking Users](rest-api/tracking-users.md)

We recommend that you get yourself acquainted with all the concepts related to [Users](../user.md) and [Events](../events.md) before proceeding. Doing so will h

Read the full guide: **[rest-api/tracking-users.md](rest-api/tracking-users.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/rest-api/tracking-users)

### [Transactional Campaign API](rest-api/transactional-campaign-api.md)

The inTrack Transactional Campaign API enables you to send critical transactional messages to your users, such as order confirmations, shipping details, payment

Read the full guide: **[rest-api/transactional-campaign-api.md](rest-api/transactional-campaign-api.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/rest-api/transactional-campaign-api)

## <a id="custom-channel"></a>Custom Channels

### [Custom Channels](custom-channel/custom-channels.md)

Custom Channel as a Provider is a feature in inTrack that allows users to create and customize their own channels for content delivery. This can be useful for b

Read the full guide: **[custom-channel/custom-channels.md](custom-channel/custom-channels.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/custom-channel/custom-channels)

### [Private Providers](custom-channel/private-providers.md)

Many businesses are hesitant to share their users' contact details with third-party platforms like inTrack. We understand this concern, so we’ve developed Priva

Read the full guide: **[custom-channel/private-providers.md](custom-channel/private-providers.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/custom-channel/private-providers)

### [Sending Push Notifications via Custom Channel](custom-channel/sending-push-notifications-via-custom-channel.md)

The process of sending push notifications through inTrack's custom channels involves the following steps. Depending on your needs, some steps may require additi

Read the full guide: **[custom-channel/sending-push-notifications-via-custom-channel.md](custom-channel/sending-push-notifications-via-custom-channel.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/custom-channel/sending-push-notifications-via-custom-channel)

### [Telegram Channel](custom-channel/telegram-provider.md)

Obtain your Telegram API token by creating a bot through `@BotFather`. Follow [this tutorial](https://core.telegram.org/bots/tutorial) for detailed steps.

Read the full guide: **[custom-channel/telegram-provider.md](custom-channel/telegram-provider.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/custom-channel/telegram-provider)

## <a id="webhook"></a>Webhook

### [How to Configure](webhook/configuration.md)

You will be able to configure URLs for different events from the Webhooks section of your  [inTrack dashboard](https://dash.intrack.ir) -> setting-> webhook. Fo

Read the full guide: **[webhook/configuration.md](webhook/configuration.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/webhook/configuration)

### [How It Works](webhook/webhook-getting-started.md)

inTrack webhooks enable you to set up HTTP callbacks for events occurring within inTrack. By configuring a webhook, inTrack can trigger a callback to a script o

Read the full guide: **[webhook/webhook-getting-started.md](webhook/webhook-getting-started.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/webhook/webhook-getting-started)

## <a id="web-view"></a>WebView integration

### [Integration Guide](web-view/implementaion-guide.md)

If your application is designed to display website pages within the app (WebView), you can use the automatic sync feature of the Web SDK to seamlessly synchroni

Read the full guide: **[web-view/implementaion-guide.md](web-view/implementaion-guide.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/web-view/implementaion-guide)

## <a id="migration"></a>Migration

### [Migrating from SDK V2 to V3](migration/migration-v2-to-v3.md)

This guide provides step-by-step instructions to help you upgrade your integration from Version 2 to Version 3 of the SDK. It outlines breaking changes, updated

Read the full guide: **[migration/migration-v2-to-v3.md](migration/migration-v2-to-v3.md)** · [View on docs.intrack.ir](https://docs.intrack.ir/docs/getting-start/migration/migration-v2-to-v3)

## For AI tools

- See [`llms.txt`](llms.txt) for a machine-readable index of all pages.
- Each `.md` file includes `docs_url` in frontmatter pointing to the live documentation.

[inTrack](https://docs.intrack.ir) · [Dashboard](https://dash.intrack.ir) · Version 3.0.0+
