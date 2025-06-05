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

## Generate Credentials File from Omnissa Intelligence Platform

- Log into the Omnissa Intelligence Platform and click the "Service Accounts" menu.

![](ws1-service-accounts-button.png)

- At the top of the page, click the "Add" button to add a new account.

![](ws1-service-accounts-add-button.png)

- Name the service account as desired.

![](ws1-service-accounts-add-namefield-button.png)

- Click "GENERATE CLIENT SECRET" button.

![](ws1-service-accounts-add-generate-button.png)

- Save the .json file to your source repository and amend the path in the Xcode Script Phase

### Common dSYM Upload Issues

- HTTP 403 Authentication Error

This most often occurs due to a missing, expired, or incorrect credentials file. Ensure that the correct credentials file is being used. This type of authentication error can also occur if the script is obtaining an incorrect app ID. Please verify in the logs that the correct app ID is being used.

- PROTOCOL_ERROR Failure during cURL Upload

It has been observed that the default cURL version which ships with current OSX builds may exhibit a known issue when uploading dSYM files. If you experience problems during the upload phase of the script, try to upgrade to a newer version. cURL version 7.69.1 and above has been verified to resolve this problem.

```C
HTTP/2 stream 1 was not closed cleanly: PROTOCOL_ERROR (err 1)
```

- The dSYM upload script causes Xcode build to fail

The script will cause a build failure by default. This behavior is possible to disable by toggling a flag inside the script itself. You may wish to change this setting if you are building without internet access, or for other similar reasons. Note that crash symbolication cannot be completed without a matching dSYM for a particular build of an app.

```C
REQUIRE_UPLOAD_SUCCESS=${REQUIRE_UPLOAD_SUCCESS:=0}
```

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