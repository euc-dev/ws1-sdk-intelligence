---
layout: page
title: Sending UEM Attributes To Intelligence SDK
hide:
  #- navigation
  - toc
---

Apps integrating the SDK should now also set an instance of type WS1UEMDataDelegate. (WS1UEMDataDelegate must be set before enabling IntelSDK). This is to publish the following UEM specific attributes serialNumber, deviceUDID, username to the Intel backend. Integration code is shown below -

### WS1UEMDataDelegate

```Swift
@objc public protocol WS1UEMDataDelegate: AnyObject {
    var serialNumber: String? { get }
    var deviceUDID: String? { get }
    var username: String? { get }
}
```

### Sample Implementation

```Swift
#import "WS1UEMAttributeKeys.h"
#import "WS1Intelligence.h"

@interface AppDelegate () <WS1UEMDataDelegate>
@end

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)
launchOptions {
    WS1Config *config = [WS1Config defaultConfig];
    [WS1Intelligence setUEMProviderDelegate:self];
    [WS1Intelligence enableWithConfig:config];
}

- (NSString *) deviceUDID {
    return [self getAppConfig:[WS1UEMAttributeKeys intelSDKDeviceUDID]];
}

- (NSString *) serialNumber {
    return [self getAppConfig:[WS1UEMAttributeKeys intelSDKSerialNumber]];
}

- (NSString *) username {
    return [self getAppConfig:[WS1UEMAttributeKeys intelSDKSerialNumber]];
}

- (NSString *) getAppConfig: (NSString*) key {
    NSDictionary<NSString*, id> *dictionary = [[NSUserDefaults standardUserDefaults] objectForKey:
[WS1UEMAttributeKeys managedAppConfigKey]];
    return [dictionary valueForKey:key];
}

@end
```

If the app does not already have access to device-udid, serial-number, username and if it is managed, then it can be fetched from UEM to be injected into IntelligenceSDK.

Within UEM, the required attributes can be set during app assignment within the ‘Application Configuration’ section as shown in the screenshot.

![](uem_application_configuration.png)

When an app is pushed with the required attributes, the attributes can be queried and injected into IntelligenceSDK as shown in the code.