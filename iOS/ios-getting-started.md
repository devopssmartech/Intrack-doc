---
title: Getting Started
version: 3.0.0+
source_path: iOS/ios-getting-started.md
github_url: https://github.com/devopssmartech/Intrack-doc/blob/main/iOS/ios-getting-started.md
github_raw_url: https://raw.githubusercontent.com/devopssmartech/Intrack-doc/main/iOS/ios-getting-started.md
---
# Getting Started

## 1.Installation

You can add the  **inTrack** SDK to your project in two ways: using CocoaPods or manually. Before you begin, ensure everything is properly configured:

* Minimum Deployment Target: The  **inTrack** iOS SDK supports a minimum deployment target of iOS 9.0.

* Xcode Version: You need to have Xcode 9.0 or later.

* Base SDK Version: Ensure you are using Base SDK iOS 10.0 or higher.

### Cocoapods

The easiest way to use  **inTrack** in your iOS project is with CocoaPods.

#### Step 1: Download CocoaPods

CocoaPods is a dependency manager for Swift and Objective-C projects. You can install CocoaPods by running the following command:

### shell

```shell
{
  `$ sudo gem install cocoapods
`
}
```



:::note

Please refer to the [CocoaPods Troubleshooting Guide](https://guides.cocoapods.org/using/troubleshooting.html) in case you have trouble using CocoaPods.🌟

:::

#### Step 2: Add a Podfil

In the terminal, navigate to your Xcode project directory and create a `Podfile` by running the following command.

### shell

```shell
{
  ` $ pod init
`
}
```



#### Step 3: Edit your Podfile

Add the following in your Podfile under your project's target.

### ruby

```ruby
{
  `target 'YourAppTarget' do
    platform :ios,'9.0'
    pod '$InTrackSDK', '3.0.3'
  end
`
}
```



#### Step 4: Install  inTrack SDK

To install the  **inTrack** SDK, navigate to your Xcode project directory and run the following command.

### shell

```shell
{
  `$ pod install
`
}
```

#### Step 5: Using xcodeproj

 Stop using your project file, YOUR-PROJECT-NAME.xcodeproj. And start using the project workspace file created by CocoaPods, YOUR-PROJECT-NAME.xcworkspace from now on.

 * Doing so will ensure that the  **inTrack** SDK is properly loaded.

### Adding inTrack SDK Manually
Also you could download inTrack.xcframework version 1.3.0 from [here](https://static1.intrack.ir/api/web/download/sdk/ios/1.3.0/$InTrack.xcframework.zip) and manually add that to your XCode project.

## 2. Initialize the SDK

Initialize  **inTrack** SDK with your `APP_KEY` and `AUTH_KEY` in the `application:didFinishLaunchingWithOptions:` of your `AppDelegate`.

### Objective-C

```swift
{
  `- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  {
    $InTrackConfig* config = $InTrackConfig.new;
    config.appKey = @"YOUR_APP_KEY";
    config.authKey = @"YOUR_AUTH_KEY";
    [$InTrack startWithConfig:config];
    /**
      YOUR CODE GOES HERE
      **/
      return YES;
  }
`
}
```

### swift

```swift
{
  `func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool
  {
    let config: $InTrackConfig = $InTrackConfig()
    config.appKey = @"YOUR_APP_KEY"
    config.authKey = @"YOUR_AUTH_KEY"
    $InTrack.initWith(config);
        /**
        YOUR CODE GOES HERE
        **/
    return true
  }
`
}
```



:::info

Check your **APP_KEY** and **AUTH_KEY**, in [inTrackPanel](https://dash.intrack.ir)->setting->SDK->Initialize, choose SDK type in **Integration Guide for** dropdown, Now your keys are in Initialization step.

:::

## 3. Support for Swift

:::note

Please follow this step if you are using `Swift` for writing your application. If you have already done this for another framework, the please skip it.

:::

#### Step 1:

Navigate to File > New and select File.... Select Objective-C File under iOS Source and click Next.

#### Step 2:
 Name the file without any extension. Click Next. (Name this file such that you can use it generically for all frameworks, and not specific to  inTrack.)

#### Step 3:
 Select the Group as your main project and target as your main app target.

 * Click Create to proceed.

#### Step 4:
You will now be prompted to create a Bridging Header.

Bridging Header is a file that makes Objective-C frameworks available to Swift and vice-versa.

Click Create Bridging Header to proceed.

#### Step 5:
Above step will create two files in your main project’s Supporting Files folder with the following name formats:
**a.**  `<YOUR_FILE_NAME>.m`
**b.** `<YOUR_PROJECT_NAME>-Bridging-Header.h`

Delete the file, `<YOUR_FILE_NAME>.m`.

#### Step 6:
Add the following imports to the `<YOUR_PROJECT_NAME>-Bridging-Header.h` file.

You only need to specify this in Objective-C even if you are using Swift for your project.

### Objective-C

```Objective-C
{
  `#import <$InTrack/$InTrack.h>
`
}
```

:::tip[**Congratulations** ]

You have now successfully integrated the **inTrack** SDK with your iOS app and are sending system events data to [your dashboard](https://dash.intrack.ir). Please note that it may take up to a few minutes for your data to reflect on the [dashboard](https://dash.intrack.ir).

:::

## Advance features

### Logging

During  **inTrack** initialization, you could enable logging. If logging is enabled, then **inTrack** SDK will print out debug messages about its internal state and encountered problems.

For receiving the **inTrack** iOS SDK's internal logs, you can set `loggerDelegate` property on the
`$InTrackConfig` object. If set, the **inTrack** iOS SDK will forward its internal logs to this delegate object.

### Objective-C

```Objective-C
{
  `config.loggerDelegate = self; //or any other object to act as $InTrackLoggerDelegate
`
}
```

### swift

```swift
{
  `config.loggerDelegate = self; //or any other object to act as $InTrackLoggerDelegate
`
}
```



`internalLog:` method declared as required in InTrackLoggerDelegate protocol will be called with log NSString.

### Objective-C

```c
{
  `// $InTrackLoggerDelegate protocol method
  -(void)internalLog:(NSString *)log
`
}
```

### swift

```swift
{
  `// $InTrackLoggerDelegate protocol method
  func internalLog(_ log: String)
`
}
```



### Crash Reporting
The **inTrack** SDK for iOS has the ability to collect crash reports, which you may examine and resolve later on the [inTrack dashboard](https://dash.intrack.ir).

To enable crash reporting with the **inTrack** SDK, configure the `enableCrashReporting` flag in the
`$InTrackConfig` object. Below is a step-by-step guide to set this up:

#### Initialize the InTrackConfig Object

### Objective-C

```c
{
  `-(BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  {
    $InTrackConfig* config = $InTrackConfig.new;
    config.appKey = @"YOUR_APP_KEY";
    config.authKey = @"YOUR_AUTH_KEY";
    config.enableCrashReporting = YES;
    [$InTrack startWithConfig:config];
      /**
      YOUR CODE GOES HERE
        **/
        return YES;
  }
`
}
```

### swift

```swift
{
  `func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptio
  [UIApplicationLaunchOptionsKey: Any]?) -> Bool
    {
      let config: $InTrackConfig = $InTrackConfig()
      config.appKey = @"YOUR_APP_KEY"
      config.authKey = @"YOUR_AUTH_KEY"
      config.enableCrashReporting = true
      $InTrack.initWith(config)
      /**
        YOUR CODE GOES HERE
          **/
          return true
  }
`
}
```



#### Crash Report Handling
With crash reporting enabled, the **inTrack** iOS SDK will generate a crash report if your application crashes due to an exception. These reports are sent to the **inTrack** server for further analysis. If a crash report cannot be delivered (e.g., due to no internet connection or server unavailability), the SDK will store the report locally and retry sending it later

### Manually Recording Handled Exceptions
In addition to automatic crash reporting, you can manually record handled exceptions. This is useful for capturing errors that you manage in your code but still want to log for analysis.

### Objective-C

```objectivec
{
  `[$InTrack recordHandledException:exception];
`
}
```

### swift

```swift
{
  `$InTrack.recordHandledException(exception)
`
}
```



By following these steps, you can ensure that all critical crash data is captured and sent to **inTrack**, allowing for thorough inspection and debugging.
