---
layout: page
title: Omnissa Intelligence SDK for iOS Release Notes
hide:
  #- navigation
  - toc
---

Updated October 28, 2024

## What's in the Release Notes

These release notes describe the new features and enhancements in each release of Omnissa IntelligenceSDK for iOS. (Sometimes called "IntelligenceSDK".) This page contains a summary of the new capabilities, issues that have been resolved, and known issues that have been reported in each release. Omnissa IntelligenceSDK for iOS is a set of tools allow iOS apps to send telemetry data to the Omnissa Intelligence backend. 



## Omnissa IntelligenceSDK for iOS 24.8.0 Release - October 2024

### Minimum Requirements

- iOS 15.0 device or iPadOS 15.0 device
- tvOS devices and app extensions are no longer supported
- visionOS for Vision Pro devices is not supported

### New Features

- Latency and Jitter information for Network DEX is now supported. No changes are required to gather this information.  If desired, the gathering can be turned off with the Privacy Configuration feature.
- A new public API named - setPrivacyConfiguration has been introduced which helps inject privacy configuration to control transmission of certain TelemetrySDK attributes. API and its documentation is like below - 

```objective-c
/// Function to help set privacy configuration for a particular telemetry type.
/// -> This configuration is ideally part of WS1 SDK's custom settings profile payload. However, IntelligenceSDK is agnostic to the configuration delivery mechanism as long
///   as the payload conforms to the required format.
/// -> A sample privacy config is like below -
/// "DEXData": {
///        "Version": 1.0,
///        "BatteryData": {
///            "DisableAll": true,
///            "AttributesToDisable": ["plugged_type", "battery_charging_rate"],
///            "EventsToDisable": []
///        },
///        "DeviceData": {
///            "DisableAll": true,
///            "AttributesToDisable": ["location_latitude", "location_longitude"],
///            "EventsToDisable": []
///        },
///        "NetworkData": {
///            "DisableAll": true,
///            "AttributesToDisable": ["jitter", "latency"],
///            "EventsToDisable": []
///        }
///    }
///
///
/// NOTE -
/// * This config is only supported for the DEX telemetry type currently. The privacy config could help  disable certain attributes / events from being reported.
///
/// - Parameters:
///   - privacyConfig: - A dictionary containing the necessary key value pairs for parsing the privacy config. If the privacy config is
///                     fetched via WS1 UEM SDK, then it is in the JSON format. Apps / SDK's are expected to fetch the value for key "DEXData"
///                     and convert it into a dictionary.
///                   - If the property "DEXData" and its value does not exist, apps are still required to call this API with nil value
///                     for the privacyConfig parameter.
///   - type: Feature for which the privacy configuration needs to be applied. Currently only DEX is supported. Other telemetry type's are ignored.
+ (void)setPrivacyConfiguration:(nullable NSDictionary<NSString *, id> *)privacyConfig forType:(WS1TelemetryType)type;
```

    - NOTES for this API -

        - This API should be called only after IntelligenceSDK initialization. 

        - Ideally IntelligenceSDK should be initialized early on in the app lifecycle process. If IntelSDK is initialized later, then it is the app’s responsibility to ensure privacy config is injected post initialization. 

        - Apps would have to read the value for the key - "DEXData", convert it into a dictionary and pass that dictionary as the parameter to the above API. The second parameter for the above API in this case is WS1TelemetryTypeDEX since this privacy config is related to the DEX feature.
        
        - For more information on this API, please visit *TODO-FILL-URL*
        
- New API introduced to export data collected through our Telemetry Features!
Users are now able to export data collected from the Telemetry Features they have Opted-Into. Use the following API to get Feature, Event, and Export formatted specific data in an Asynchronous fashion! 

```objective-c
/// Function to asynchronously export telemetry data for enabled features. The only supported features for exporting data are - DEX and ZeroTrust
/// -> A telemetry feature must be enabled before data can be queried else the exported data could be stale or nil.
/// -> See `setOptInStatusForType` API for enabling / disabling features.
/// -> If `telemetryType` is passed as `WS1TelemetryTypeAll` then all enabled telemetry data (`DEX` or `ZeroTrust` or both) is exported.
///
/// - Parameters:
///   - telemetryType: The telemetry feature data to be exported if available. One of  `DEX` or `ZeroTrust`. `Application` is `not` supported.
///                    Pass `WS1TelemetryTypeAll` to export all telemetry enabled feature data i.e. `DEX` / `ZeroTrust`.
///   - dataFormatType: The format of the data to be exported. Currently, the only supported type is JSON.
///   - dataCategoryType: Category of the data to be exported. It can be one of `AttributeData`, `EventData` or `AllData`
///   - completionHandler: completion handler which is called with the relevant data.
+ (void)exportTelemetryFeatureData:(WS1TelemetryType)telemetryType
                          ofFormat:(WS1TelemetryExportDataFormatType)dataFormatType
                      withCategory:(WS1TelemetryExportDataCategoryType)dataCategoryType
                     andCompletion:(void (^)(NSString* data))completionHandler;
```

### Resolved Issues

- An issue that was causing a failure when rendering webpages (especially SAP SuccessFactors) has been fixed. If monitoring WKWebView was disabled, please enable it again.

### Known Issues

none


## Omnissa IntelligenceSDK for iOS 24.6.0 Release - August 2024

### Minimum Requirements

- iOS 15.0 device or iPadOS 15.0 device
- tvOS devices and app extensions are no longer supported
- visionOS for Vision Pro devices is not supported

### New Features

The following APIs are now deprecated:

```objective-c

   + (void)setOptOutStatus:(BOOL)status __deprecated_msg("Use setOptInStatusForType:type:status instead");
   + (BOOL)getOptOutStatus __deprecated_msg("Use getOptInStatusForType:type instead");
```

Users are now able to individually opt in for Application or DEX telemetry data collection.

```objective-c

   typedef NS_ENUM(NSInteger, WS1TelemetryType) {
       WS1TelemetryTypeApplication = 0,
       WS1TelemetryTypeDEX,
   };
   
   /*! Sets the Opt-In status for telemetry instrumentations, currently the two
    * options are Application or DEX. Setting a telemetry instrumentation Opt-In to YES enables the
    * collection of telemetry data.
    * The default for Application is YES
    * The default for DEX is NO
    * @param type Application or DEX
    * @param status new Opt-In status
    */
   + (void)setOptInStatusForType:(WS1TelemetryType)type andStatus:(BOOL)status;
   
   /*! Returns the current Opt-In status for telemetry instrumentation type
    * @param type Application or DEX
    */
   + (BOOL)getOptInStatusForType:(WS1TelemetryType)type;
```

- Important Notes when using the above API’s
   - Always enable IntelSDK first 
      i.e. start IntelSDK first before interacting with the APIs. 
      Interacting with the API’s without enabling DEX will not have an impact on feature enablement.   
   - DEX is disabled by default.
      So if the custom settings do not exist or if it is disabled, please out out of DEX using the above API.

- Location Data
   - IntelligenceSDK can now request the location_longitude and location_latitude attributes for the device. This is true only if DEX is enabled.
   - The app must have the resources or entitlements needed to request location permission from the user, and the user must grant permission to use the device location. To prevent changing app behavior, IntelSDK will not trigger the request for location permission. 
   
   - Apple Privacy Information
      - The IntelligenceSDK’s privacy manifest has been updated to show it collects precise location information.
      - App developers will need to include this location declaration in App Store submissions.
      
   - Limitations
      - DEX native code will request a location update from iOS once every 5 minutes, when permissions allow.  Each location result is cached for a maximum of 5 minutes. 
      - Background location
         - DEX stops requesting locations when the app is put into the background. 
         - DEX starts requesting locations again when the app returns to the foreground. 
         - DEX does not interact location updates portion of the background mode capability detailed in Handling location updates in the background 


### Resolved Issues

none

### Known Issues

none

____

## Omnissa IntelligenceSDK for iOS 24.3.0 Release - April 2024

### Minimum Requirements

- iOS 15.0 device or iPadOS 15.0 device
- tvOS devices and app extensions are no longer supported
- visionOS for Vision Pro devices is not supported

### New Features

- IntelSDK is now shipped with Privacy Manifest as per Apple's recommendations. 

### Resolved Issues

- Computing upload / download speeds are disabled.
- totalDiskSpace and freeDiskSpace attribute computation is disabled to comply with Apple's privacy manifest requirements.

### Known Issues

none

____

## Omnissa IntelligenceSDK for iOS 24.1.0 Release - March 2024

### Minimum Requirements

- iOS 15.0 device or iPadOS 15.0 device
- tvOS devices and app extensions are no longer supported
- visionOS for Vision Pro devices is not supported

### New Features

- A fix supports the instrumentation and use of the newer `WKNavigationDelegate` method introduced by Apple in iOS 13.  The app can supply the new version of decidePolicyForNavigationAction, the old version, or can supply neither. 
  - Apple Documentation: [https://developer.apple.com/documentation/webkit/wknavigationdelegate/3223382-webview](https://developer.apple.com/documentation/webkit/wknavigationdelegate/3223382-webview)
- SDK imports have to be renamed from `WS1Intelligence` to `WS1IntelligenceSDK`
- **dsym upload shell script has been updated to parse the latest service account json file. Please re-create and re-download the service account json file to support the dsym upload/parsing scripts. Instructions to generate the service account json is here - 
[link](install-ios.md#generate-credentials-file-from-workspace-one-intelligence-platform)
- WS1IntelligenceSDK enable API takes in a WS1 config object outside of the AppID. This config object should be used to enable DEX and inject all entitlements that an app supports as shown below:

```Swift
let config = WS1Config.default()
config.enableMachExceptionHandling = true
// enable DEX for DEX capabilities
config.enableDEX = true
// Inject all entitlements that App supports that SDK requires.
config.entitlements = [.wifi_info()]
 
WS1Intelligence.enable(withAppID: "App ID", config: config)
```

- A list of all entitlements / permissions that WS1IntelligenceSDK requires is listed below. If the entitlements / permissions are not given / available then those attributes are not sent to the Intel backend.

**Permissions / Entitlements Required**

| Attribute | Platform | Entitlements | Permissions | 
| --- | --- | --- | --- |
| bluetooth_connected | MacOS	| Bluetooth	|   |
| ssid | iOS | wifi_info | Location |
| bssid | iOS | wifi_info | Location |
| wifi_signal_strength | iOS | wifi_info | Location |

- IntelSDK is also compatible with M1/M2 machines.
- Introduced more modern external distribution options:
  - XCFramework
  - Swift Package Manager
- Apps integrating the SDK should now also set an instance of type `WS1UEMDataDelegate`. (**WS1UEMDataDelegate must be set before enabling IntelSDK**). This is to publish the following UEM specific attributes **serialNumber, deviceUDID, username** to the Intel backend. Integration code is shown below:
  - WS1UEMDataDelegate

```JAVA
@objc public protocol WS1UEMDataDelegate: AnyObject {
    var serialNumber: String? { get }
    var deviceUDID: String? { get }
    var username: String? { get }
}
```

- Sample Implementation

```JAVA
#import "WS1UEMAttributeKeys.h"
#import "WS1Intelligence.h"
 
 
@interface AppDelegate () <WS1UEMDataDelegate>
@end
 
@implementation AppDelegate
 
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
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
    NSDictionary<NSString*, id> *dictionary = [[NSUserDefaults standardUserDefaults] objectForKey: [WS1UEMAttributeKeys managedAppConfigKey]];
    return [dictionary valueForKey:key];
}
 
@end
```

### Resolved Issues

- Fixed a bug that could crash the app if the user reset the device's clock to an earlier time than the last use of the app.
- Fixed a bug that caused a crash if a URL specified by the app was nil.
- improved the way the "App Start" userflow is automatically generated. This should give more reliable times in the reports.
- Resolved an issue when instrumenting WKWebView if loaded from storyboard.

### Known Issues

- This version currently does not support Apple Privacy [manifest](https://developer.apple.com/documentation/bundleresources/privacy_manifest_files/describing_data_use_in_privacy_manifests) policy.
