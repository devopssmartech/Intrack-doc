---
title: Push Messaging
version: 3.0.0+
source_path: iOS/push-messaging.md
docs_url: https://docs.intrack.ir/docs/getting-start/iOS/push-messaging
---
# Push Messaging

:::warning

Please ensure that you have completed all the platform integration steps listed under [GettingStarted](ios-getting-started) before proceeding.

:::

:::info WebEngage Integration
If you are using **WebEngage** for push notifications, please follow the steps in [Integrating Push Notifications with WebEngage](push-messaging#integrating-push-notifications-with-webengage) before proceeding with this guide.
:::

**inTrack** push messaging can be integrated using [Apple Push Notification service](https://developer.apple.com/go/?id=push-notifications). To integrate **inTrack** in your iOS application, you need to be enrolled in [Apple Developer Program](https://developer.apple.com/programs/).

In order to send push notifications to your iOS users, you will need to upload either your APNs Auth Key or APNs Certificate. We strongly recommend that you create and upload APNs Auth Key for the following reasons:

* One auth key can be used for all your apps - hence there is no complication of maintaining different certificates for different apps

* There is no need to re-generate the push certificate every year once it expires.

If you already support push notifications in your app, skip to the section below about uploading your auth key. Note that you may still need to enable background modes.

## Uploading your auth key

### 1. Enable Push and Background Modes Capabilities

Here's how you can go about it:

**Step 1:** Enter Project Navigator view.

**Step 2:** Select your main app target from the expanded sidebar or from the dropdown menu, then select the Capabilities tab.

**Step 3:** If Push Notifications isn't enabled, click the switch to add the "Push Notifications" entitlement to your app. If you are using Xcode 8 or above, ensure that a YOUR-APP-NAME.entitlements file has been added to your project.

**Step 4:** If Background Modes is not enabled, click the switch and then tick the Remote notifications checkbox. WebEngage uses background modes to track when push messages are received on a user's device before the message is opened.

### 2. Create APNs Auth Key

The documentation below covers the process of creating the APNs Auth Key to send push notifications to your users.

Follow these steps to create an APNs Auth Key:

**Step 1:** Log in to the [Apple Developer Consol](https://developer.apple.com/account/) and go to [Certificates, Identifiers & Profiles](https://developer.apple.com/account/resources).

**Step 2:** Create a new Auth Key by clicking on the + button on the top right corner.

**Step 3:** Add a Key Name, and select APNs.

**Step 4:** Click the Confirm button.

**Step 5:** On this page, you will be able to download your Auth Key file. Please do not rename this file, and upload it as is to the [inTrack dashboard](https://dash.intrack.ir).

### 3. Upload APNs Auth Key

You need to upload the APNs Auth Key on **inTrack** in order to enable  inTrack to send push notifications to your users on your behalf.

The documentation below covers the process of uploading the APNs Auth Key to send push notifications to your users.

**Step 1:** Log in to your [inTrack dashboard](https://dash.intrack.ir) and navigate to settings > Channels.

**Step 2:** In Push tab, under the iOS section, select Auth Key as APNs Authentication Type. Upload the APNs Auth Key file that you had downloaded earlier, and paste your Team ID and your app’s Bundle ID. Your app’s Bundle ID can be found in Xcode.

**Step 3:** Select the default push mode for sending push notifications i.e. Production or Development and then Click Save.

## Initialize Push

Using **inTrack** Push Notifications on iOS apps is pretty straightforward. After initialze **inTrack**, you'll need to initalize push notifications by calling `initPush` method, The **inTrack** iOS SDK will automatically handle the rest. No need to call any other method for registering when a device token is generated, or a push notification is received.
please note that you also need to ask for user's permission for push notifications, if you didn't already get the permission, you can use **inTrack** helpers method.

### Objective-C

```objectivec
{`
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  {
    $InTrackConfig* config = $InTrackConfig.new;
    config.appKey = @"YOUR_APP_KEY";
    config.authKey = @"YOUR_AUTH_KEY";
    config.enableCrashReporting = YES;
    [$InTrack startWithConfig:config];
    $InTrackPushConfig* pushConfig = $InTrackPushConfig.new;

    // pushConfig.doNotShowAlertForNotifications = NO;
    [$InTrackPush startWithConfig:pushConfig];
    //Ask for user's permission for Push Notifications (not necessarily here)
    //You can do this later at any point in the app after starting $InTrack push service.
    [$InTrack askForNotificationPermission];

    /**
      YOUR CODE GOES HERE
      **/

    return YES;
}
`}
```

### swift

```swift
{`func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
{
    let config: $InTrackConfig = $InTrackConfig()
    config.appKey = "YOUR_APP_KEY"
    config.authKey = "YOUR_AUTH_KEY"
    $InTrack.initWith(config)
    // initialize push
    let pushConfig: $InTrackPushConfig = $InTrackPushConfig()
    // pushConfig.doNotShowAlertForNotifications = false
    $InTrack.initPush(pushConfig)
    //Ask for user's permission for Push Notifications (not necessarily here)
    //You can do this later at any point in the app after starting $InTrack push service.
    $InTrack.askForNotificationPermission()
    /**
        YOUR CODE GOES HERE
    **/

    return true
}`}
```

:::note

Please make sure you do not set UNUserNotificationCenter.currentNotificationCenter's delegate manually, as inTrack iOS SDK will be acting as the delegate.

:::

### Notification Permission with Preferred Types and Callback

**inTrack** askForNotificationPermission method, simply asks for a user's permission for all available notification types. However, if you need to specify which notification types your app will use (alert, badge, sound) or if you need a callback to see a user's response to the permission dialog, you can use the `askForNotificationPermissionWithOptions:completionHandler:` method.

### Objective-C

```objectivec
{`- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    $InTrackConfig* config = $InTrackConfig.new;
    config.appKey = @"YOUR_APP_KEY";
    config.authKey = @"YOUR_AUTH_KEY";
    config.enableCrashReporting = YES;
    [$InTrack startWithConfig:config];
    $InTrackPushConfig* pushConfig = $InTrackPushConfig.new;
    // pushConfig.doNotShowAlertForNotifications = NO;
    [$InTrackPush startWithConfig:pushConfig];
    //Ask for user's permission for Push Notifications (not necessarily here)
    //You can do this later at any point in the app after starting $InTrack push service.
    [$InTrack askForNotificationPermission];

    /**
        YOUR CODE GOES HERE
    **/

    return YES;
}`}
```

### swift

```swift
{`let authorizationOptions: UNAuthorizationOptions = [.badge, .alert, .sound]
$InTrack.askForNotificationPermission(options: authorizationOptions, completionHandler:
{ (granted: Bool, error: Error?) in
    print("granted: \(granted)")
    print("error: \(error)")
})`}
```

### Rich Push Notifications

:::warning

Please ensure that you have completed all the platform integration steps listed under [GettingStarted](ios-getting-started) before proceeding.

:::

Following requires iOS 10.0+

#### 1. Create Notification Service Extension
Here's how you can go about it:

**Step 1:** In Xcode, navigate to File > New > Target and select Notification Service Extension.

**Step 2:** Click Next, fill out the Product Name  as you wish (for example: inTrackNSE), and click Finish.

**Step 3:** Click Activate on the prompt shown to activate the service extension. Xcode will now create a new top-level folder in your project with the name, `$InTrackNotificationService` (NotificationService.swift in Swift projects).

#### 2. Create Notification Content Extension

Here's how you can go about it:

Step 1: In Xcode, navigate to File > New > Target and select Notification Content Extension.

Step 2: Click Next, fill out the Product Name as NotificationViewController, and click Finish

Step 3: Click Activate on the prompt shown to activate the content extension. Xcode will now create a new top-level folder in your project with the name NotificationViewController

Here's how you can update service and content extension classes:

### Objective-C

```objectivec
{`#import "$InTrackNotificationService.h"

- (void)didReceiveNotificationRequest:(UNNotificationRequest *)request withContentHandler:(void (^)(UNNotificationContent * _Nonnull))contentHandler {
    self.contentHandler = contentHandler;
    self.bestAttemptContent = [request.content mutableCopy];
    //delete existing template code, and add this line
    [$InTrackNotificationService didReceiveNotificationRequest:request withContentHandler:contentHandler];
}`}
```

### swift

```swift
{`override func didReceive(_ request: UNNotificationRequest, withContentHandler contentHandler: @escaping (UNNotificationContent) -> Void)
{
    self.contentHandler = contentHandler
    bestAttemptContent = (request.content.mutableCopy() as? UNMutableNotificationContent)
    //delete existing template code, and add this line
    $InTrackNotificationService.didReceive(request, withContentHandler: contentHandler)
}`}
```

:::note

Please ensure you also configure the App Transport Security setting in the extension's Info.plist file just as with the main application. Otherwise, media attachments from non-https sources cannot be loaded.

:::

:::note

 Please ensure you check that the Deployment Target version of the extension target is 10, not 10.3 (or whatever minor version Xcode set automatically). Otherwise, users running iOS versions lower than the Deployment Target value will not be able to get rich push notifications.

:::

### Disabling Alerts Shown by Notifications

To disable messages from automatically being shown by the inTrack while the app is in the foreground, you can set the `doNotShowAlertForNotifications` flag on the `$InTrackPushConfig` object. If set, no message will be displayed by using the default system UI in the app, but push-open events will be recorded automatically.

### Manually Handling Notifications

If you would like to do additional custom work when a push notification is received, all you need to do is implement the default push-related methods in your application delegate (e.g. AppDelegate.m). After finishing its internal work, the **inTrack** iOS SDK will push forward the related method calls to the default implementations on the application delegate.

Please ensure you do not set `theUNUserNotificationCenter`. currentNotificationCenter's delegate manually, as the **inTrack** iOS SDK will be acting as the delegate. All you need to do is directly add `theUNUserNotificationCenterDelegate` methods to your application delegate class.

:::info

You can checked push notification system events [here](../events#push-system-events)

:::

## Integrating Push Notifications with WebEngage

This document outlines how to enable **inTrack** push notifications on iOS when
you're also using WebEngage.

:::note

Important: Do not call `$InTrack.askForNotificationPermission(...) ` or `$InTrack.initPush(pushConfig)` in your app.

:::

### 1.Add Notification Service to Service Extension Target

* `$InTrackNotificationService.m`
* `$InTrackNotificationService.h`

**Note:** Please contact **InTrack Customer Support** to obtain these files.

If your project uses Swift: Add this line to your Bridging Header:
`#import $InTrackNotificationService.m`

### 2.Update Notification Service Class to Support Rich Push

Modify your Notification Service Extension as follows:

### Objective-C

```objectivec
{`#import "WEXPushNotificationService.h"
#import <UserNotifications/UserNotifications.h>
#import "$InTrackNotificationService.h"

@interface NotificationService : WEXPushNotificationService
@end

@implementation NotificationService

- (void)didReceiveNotificationRequest:(UNNotificationRequest *)request
                  withContentHandler:(void (^)(UNNotificationContent * _Nonnull))contentHandler {

    NSDictionary *userInfo = request.content.userInfo;
    NSDictionary *$intrackPl = userInfo[@"c"];

    if ($intrackPl != nil && [$intrackPl isKindOfClass:[NSDictionary class]] && $intrackPl.count > 0) {
        [$InTrackNotificationService didReceive:request withContentHandler:contentHandler];
    } else {
        [super didReceiveNotificationRequest:request withContentHandler:contentHandler];
    }
}
@end`}
```

### swift

```swift
{`class NotificationService: WEXPushNotificationService {
    override func didReceive(_ request: UNNotificationRequest, withContentHandler contentHandler: @escaping (UNNotificationContent) -> Void) {
        let userInfo = request.content.userInfo
        let $intrackPl = userInfo["c"] as? [String: Any] ?? [:]
        if !$intrackPl.isEmpty {
            $InTrackNotificationService.didReceive(request, withContentHandler: contentHandler)
        } else {
            super.didReceive(request, withContentHandler: contentHandler)
        }
    }
}`}
```

### 3.Configure WebEngage in AppDelegate

In your `AppDelegate.swift`, set up WebEngage as shown:

### Objective-C

```objectivec
{`#import "AppDelegate.h"
#import <UserNotifications/UserNotifications.h>
#import "GeneratedPluginRegistrant.h"
#import <WebEngage/WebEngage.h>

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [GeneratedPluginRegistrant registerWithRegistry:self];
    [WebEngage sharedInstance].pushNotificationDelegate = self.bridge;

    [[WebEngage sharedInstance] application:application
                 didFinishLaunchingWithOptions:launchOptions
                          notificationDelegate:self.bridge];

    [[UIApplication sharedApplication] registerForRemoteNotifications];
    [UNUserNotificationCenter currentNotificationCenter].delegate = self;

    return [super application:application didFinishLaunchingWithOptions:launchOptions];
}

@end`}
```

### swift

```swift
{`override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    GeneratedPluginRegistrant.register(with: self)
    WebEngage.sharedInstance().pushNotificationDelegate = self.bridge
    WebEngage.sharedInstance().application(application, didFinishLaunchingWithOptions: launchOptions, notificationDelegate: self.bridge)
    UIApplication.shared.registerForRemoteNotifications()
    UNUserNotificationCenter.current().delegate = self
    return super.application(application, didFinishLaunchingWithOptions: launchOptions)
}`}
```

### 4.Send Device Token to SDK

### Objective-C

```objectivec
{`#import "AppDelegate.h"
#import <$InTrack/$InTrack.h>

@implementation AppDelegate

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *)deviceToken {
    [$InTrack onTokenRefresh:deviceToken];
}
@end`}
```

### swift

```swift
{`override func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {
    $InTrack.onTokenRefresh(deviceToken)
}`}
```

### 5.Handle Foreground Push Notifications

### Objective-C

```objectivec
{`#import "AppDelegate.h"
#import <UserNotifications/UserNotifications.h>

@implementation AppDelegate

- (void)userNotificationCenter:(UNUserNotificationCenter *)center willPresentNotification:(UNNotification *)notification withCompletionHandler:(void (^)(UNNotificationPresentationOptions options))completionHandler {
    completionHandler(UNNotificationPresentationOptionAlert | UNNotificationPresentationOptionSound);
}

@end`}
```

### swift

```swift
{`override func userNotificationCenter(_ center: UNUserNotificationCenter, willPresent notification: UNNotification, withCompletionHandler completionHandler: @escaping (UNNotificationPresentationOptions) -> Void) {
    completionHandler([.alert, .sound])
}`}
```

### 6.Handle Background Push Notifications

### Objective-C

```objectivec
{`#import "AppDelegate.h"
#import <UIKit/UIKit.h>

@implementation AppDelegate

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
      fetchCompletionHandler:(void (^)(UIBackgroundFetchResult))completionHandler {
    completionHandler(UIBackgroundFetchResultNewData);
}

@end`}
```

### swift

```swift
{`override func application(_ application: UIApplication, didReceiveRemoteNotification userInfo: [AnyHashable: Any], fetchCompletionHandler completionHandler: @escaping (UIBackgroundFetchResult) -> Void) {
    completionHandler(.newData)
}`}
```

### 7.Send Click Event and Handle Notification Clicks

### Objective-C

```objectivec
{`
#import "AppDelegate.h"
#import <UserNotifications/UserNotifications.h>

@implementation AppDelegate

- (void)userNotificationCenter:(UNUserNotificationCenter *)center
didReceiveNotificationResponse:(UNNotificationResponse *)response
         withCompletionHandler:(void (^)(void))completionHandler {

    NSDictionary *userInfo = response.notification.request.content.userInfo;
    NSDictionary *$intrackPayload = userInfo[@"c"];
    NSString *notificationID = $intrackPayload[@"id"];

    if (!$intrackPayload || !notificationID) {
        completionHandler();
        return;
    }

    NSInteger buttonIndex = 0;
    NSString *url = nil;

    if ([response.actionIdentifier isEqualToString:UNNotificationDefaultActionIdentifier]) {
        url = $intrackPayload[@"link"];
    } else if ([response.actionIdentifier hasPrefix:@"$InTrackActionIdentifier"]) {
        NSString *indexString = [response.actionIdentifier stringByReplacingOccurrencesOfString:@"$InTrackActionIdentifier" withString:@""];
        buttonIndex = [indexString integerValue];

        NSArray *buttons = $intrackPayload[@"buttons"];
        if ([buttons isKindOfClass:[NSArray class]] && buttons.count >= buttonIndex) {
            NSDictionary *button = buttons[buttonIndex - 1];
            url = button[@"l"];
        }
    }

    // Send click event
    NSString *eventUrl = [NSString stringWithFormat:@"https://api.$intrack.ir/api/sdk/track/clicked?token=%@&button=%ld&url=%@", notificationID, (long)buttonIndex, url ?: @""];
    NSURL *urlObj = [NSURL URLWithString:eventUrl];
    NSURLSessionDataTask *task = [[NSURLSession sharedSession] dataTaskWithURL:urlObj completionHandler:^(NSData *data, NSURLResponse *response, NSError *error) {
        // Handle response if needed
    }];
    [task resume];

    if (url) {
        [self openURL:url];
    }

    completionHandler();
}

@end`}
```

### swift

```swift
{`override func userNotificationCenter(_ center: UNUserNotificationCenter, didReceive response: UNNotificationResponse, withCompletionHandler completionHandler: @escaping () -> Void) {
    let userInfo = response.notification.request.content.userInfo
    guard let $intrackPayload = userInfo["c"] as? [String: Any], let notificationID = $intrackPayload["id"] as? String else {
        completionHandler()
        return
    }

    var buttonIndex = 0
    var url: String?

    if response.actionIdentifier == UNNotificationDefaultActionIdentifier {
        url = $intrackPayload["link"] as? String
    } else if response.actionIdentifier.hasPrefix("$InTrackActionIdentifier") {
        let indexString = response.actionIdentifier.replacingOccurrences(of: "$InTrackActionIdentifier", with: "")
        if let index = Int(indexString) {
            buttonIndex = index
            if let buttons = $intrackPayload["buttons"] as? [[String: Any]],
               buttons.indices.contains(buttonIndex - 1) {
                url = buttons[buttonIndex - 1]["l"] as? String
            }
        }
    }

    let eventUrl = URL(string: "https://api.$intrack.ir/api/sdk/track/clicked?token=\(notificationID)&button=\(buttonIndex)&url=\(url ?? "")")!
    let task = URLSession.shared.dataTask(with: eventUrl) { data, response, error in
        // Handle response if needed
    }
    task.resume()

    if let urlString = url {
        openURL(urlString)
    }

    completionHandler()
}`}
```
