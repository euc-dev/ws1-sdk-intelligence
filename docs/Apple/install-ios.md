---
layout: page
title: Installing Omnissa Intelligence SDK for iOS
hide:
  #- navigation
  - toc
---

!!!Note
    Cocoapods is no longer supported as a means of installing Omnissa Intelligence SDK. Please use Swift Package Manager as described below.
    The Crittercism framework is no longer supported. Use the WS1IntelligenceSDK framework.

Add the Omnissa Intelligence SDK Swift Package Manager (SPM) package from this URL:
    https://github.com/euc-releases/ws1-intelligencesdk-sdk-ios.git

![Image of Xcode's Swift Package Manager screen while adding 'Omnissa Intelligence SDK for iOS'](./add_ws1intelligencesdk_spm.png)

## Basic Setup

Obtain your app ID from Omnissa Intelligence platform. Please check [Omnissa Intelligence SDK Data for Apps](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelIntelligenceSDKApps.html) for more information. 

### 1. Import the Omnissa Intelligence SDK header.

!!!Note
    SDK imports must now be from the “WS1IntelligenceSDK” module rather than the “WS1Intelligence” module.

For Objective-C apps, import the Omnissa Intelligence SDK header in your application delegate’s implementation file. 

```objective-c
#import <WS1IntelligenceSDK/WS1Intelligence.h>
```

For Swift applications, place this import in your bridging-header.h file.

```Swift
import WS1IntelligenceSDK
```

### 2. Enable Omnissa Intelligence SDK.

Call `enableWithAppID` in your AppDelegate’s `application:didFinishLaunchingWithOptions:` method.

```C
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
  [WS1Intelligence enableWithAppID:@"YOUR APP ID GOES HERE"];
  // other code for your app
  return YES;
}
```

```Swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
  WS1Intelligence.enable(withAppID: "YOUR APP ID GOES HERE")
  // other code for your app
  return true
}
```

Register your app at the Omnissa Intelligence portal to get an App ID to be used in place of "YOUR APP ID GOES HERE".

Your app is now integrated with Omnissa Intelligence! Additional features require adding more code to your project.

## Interoperability with other SDKs

Recommended initialization orders:

### Crashlytics:

Omnissa Intelligence SDK should be initialized before Crashlytics

Objective-C

```objective-c
[WS1Intelligence enableWithAppID:@"YOUR APP ID GOES HERE"];

// Initialize Crashlytics
```

Swift

```Swift
WS1Intelligence.enable(withAppID: "YOUR APP ID GOES HERE")

// Initialize Crashlytics
```

### AppDynamics:

AppDynamics should be initialized before Omnissa Intelligence SDK

```C
Objective-C

// Initialize AppDynamics

[WS1Intelligence enableWithAppID:@"YOUR APP ID GOES HERE"];
```

Swift

```Swift
// Initialize Crashlytics

WS1Intelligence.enable(withAppID: "YOUR APP ID GOES HERE")
```

### New Relic:

Omnissa Intelligence SDK should be initialized before New Relic

Objective-C

```C
[WS1Intelligence enableWithAppID:@"YOUR APP ID GOES HERE"];

// Initialize New Relic
```

Swift

```Swift
WS1Intelligence.enable(withAppID: "YOUR APP ID GOES HERE")

// Initialize New Relic
```