---
title: In-app Messaging
version: 3.0.0+
source_path: iOS/ios-inapp-messaging.md
docs_url: https://docs.intrack.ir/docs/getting-start/iOS/ios-inapp-messaging
---
# In-app Messaging

## Initialization

you can enable In App messaging service by calling `enableInAppMessaging` on InTrackConfig object during **inTrack** initialization.

:::warning

for using In App messaging you should use **inTrack** sdk (version 2.6.5 or higher).

:::

### Objective-C

```objectivec
{
  `$InTrackConfig* config = $InTrackConfig.new;
  config.appKey = @"YOUR_APP_KEY";
  config.authKey = @"YOUR_AUTH_KEY";
  config.enableInAppMessaging = YES;
  [$InTrack startWithConfig:config];
`
}
```

### swift

```swift
{
  `let config: $InTrackConfig = $InTrackConfig()
  config.appKey = @"YOUR_APP_KEY"
  config.authKey = @"YOUR_AUTH_KEY"
  config.enableInAppMessaging = true
  $InTrack.initWith(config)
`
}
```



## Custom Screen tracking

Screens are mobile equivalent of web pages, which can have associated properties. **inTrack** SDK allows you to tag whenever a user sees a screen in your iOS pages.These tags allow you to pinpoint screens in your app where you would later render in-app messages using [inTrack dashboard](https://dash.intrack.ir). Note that screens are only usable for targeting in-app engagements.
you can manually tell **inTrack** about user navigation by calling **inTrack**'s `customNavigatedTo` method:

### Objective-C

```objectivec
{
  `[$InTrack customNavigateTo:@"YOUR_SCREEN_NAME" data:nil];
`
}
```

### swift

```swift
{
  `$InTrack.customNavigate(to:"YOUR_SCREEN_NAME", data: nil)
`
}
```



## Tracking Screen Data

Every screen can be associated with some contextual data, which can be part of the targeting rule for in-app messages. You can set the screen name and data in a single API call as shown below.

### Objective-C

```objectivec
{
  `NSMutableDictionary *customScreenData = [NSMutableDictionary dictionary];
  customScreenData[@"category"] = @"YOUR_PRODUCT_CATEGORY";
  customScreenData[@"price"] = @100;
  [$InTrack customNavigateTo:@"YOUR_SCREEN_NAME" data:customScreenData];
`
}
```

### swift

```swift
{
  `var customScreenData: [String: Any] = [:]
  customScreenData["category"] = "YOUR_PRODUCT_CATEGORY"
  customScreenData["price"] = 100
  $InTrack.customNavigate(to: "YOUR_SCREEN_NAME", data: customScreenData)
`
}
```



## Callbacks

Callbacks are useful for understanding the lifecycle stages of **inTrack** messages. All **inTrack** callbacks are called on the main thread.
If you want to get notified about user reactions to Messages, first you should implement **InTrackInAppMessagingDisplayDelegate**:

### Objective-C

```objectivec
{
    `@interface AppDelegate : UIResponder <UIApplicationDelegate, $InTrackInAppMessagingDisplayDelegate>
     @end

     @implementation AppDelegate
     - (void)messageClicked:($InTrackInAppMessage *)inAppMessage {
         NSLog(@"[MESSAGE CLICKED ------>] @", inAppMessage.messageId ?: @"");
      }
    - (void)messageDismissed:($InTrackInAppMessage *)inAppMessage {
      NSLog(@"[MESSAGE DISMISSED ------>] @", inAppMessage.messageId ?: @"");
      }
    - (void)messageTriggered:($InTrackInAppMessage *)inAppMessage {
        NSLog(@"[MESSAGE TRIGGERED ------>] @", inAppMessage.messageId ?: @"");
      }
    @end
`
}
```



### swift

```swift
{
  `@main
  class AppDelegate: UIResponder, UIApplicationDelegate, $InTrackInAppMessagingDisplayDelegate {

    func messageClicked(_ inAppMessage: $InTrackInAppMessage?) {
        NSLog("%@%@", "[MESSAGE CLICKED ------>]", inAppMessage?.messageId ?? "");
    }

    func messageDismissed(_ inAppMessage: $InTrackInAppMessage?) {
        NSLog("%@%@", "[MESSAGE DISSMISSED ------>]", inAppMessage?.messageId ?? "");
    }

    func messageTriggered(_ inAppMessage: $InTrackInAppMessage?) {
        NSLog("%@%@", "[MESSAGE TRIGGERED ------>]", inAppMessage?.messageId ?? "");
    }
  }
`
}
```



**on Message Trigger:** This callback is invoked before the in-app message is shown to your users.

**on Message Click:** This callback is invoked when the user clicks the message.

**on Message Dismiss:** This callback is invoked when the user dismisses the messages.

and set `inAppDisplayDelegate = self` as shown below:

### Objective-C

```objectivec
{
  `$InTrackConfig* config = $InTrackConfig.new;
    config.appKey = @"YOUR_APP_KEY";
    config.authKey = @"YOUR_AUTH_KEY";
    config.enableInAppMessaging = YES;
    config.inAppDisplayDelegate = self;
    [$InTrack startWithConfig:config];
`
}
```

### swift

```swift
{
  `let config: $InTrackConfig = $InTrackConfig()
  config.appKey = @"YOUR_APP_KEY"
  config.authKey = @"YOUR_AUTH_KEY"
  config.enableInAppMessaging = true
  config.inAppDisplayDelegate = self
  $InTrack.initWith(config)
`
}
```



:::info

You can checked InApp system events [here](../events#inapp-system-events)

:::
