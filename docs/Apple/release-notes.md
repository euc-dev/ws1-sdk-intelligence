---
layout: page
title: Omnissa Intelligence SDK for iOS Release Notes
hide:
  #- navigation
  - toc
---

Updated June 5, 2025

## What's in the Release Notes

These release notes describe the new features and enhancements in each release of Omnissa IntelligenceSDK for iOS. (Sometimes called "IntelligenceSDK".) This page contains a summary of the new capabilities, issues that have been resolved, and known issues that have been reported in each release. Omnissa IntelligenceSDK for iOS is a set of tools allow iOS apps to send telemetry data to the Omnissa Intelligence backend. 


## Omnissa IntelligenceSDK for iOS 25.4.0 Release - June 2025

### Minimum Requirements

- Devices running iOS 16.0 or iPadOS 16.0 or newer.
- WS1SDK version 25.04.0 or newer is required for the two to interact. Note that WS1SDK requires the `fips.xcframework` that is included in its release.
- not supported:
   - tvOS devices
   - app extensions
   - visionOS for Vision Pro devices

### New Features

- New DEX Telemetry event for iOS: **Critical Battery Level**
   - This event is triggered when the device reaches the following battery thresholds while the SDK is running:
      - Reports once when the device reaches a battery level between 16-20%
      - Reports once when the device reaches a battery level between 11-15%
      - Reports on every battery level between 0-10%

   When your iOS device is charging, these events will not be sent, even if the device is in the specified range.
   Device charging will reset the ability to send for all given sections. For example: If we have already sent an event for values between 16-20%, charging followed by discharging will allow another event to be sent for this battery level range.

- New DEX Telemetry attribute for iOS: **Percent Asleep**
   - Name: percent_asleep
   - Represents the duration percentage that the device has been asleep since it was booted (on a scale from 0 to 100).
   
- New DEX Telemetry attribute for iOS: **Percent Awake**
   - Name: percent_awake
   - Represents the duration percentage that the device has been awake since it was booted (on a scale from 0 to 100).

### Resolved Issues

- Creation of the automatic `WS1IntelligenceSDK-Enable-Completed` breadcrumb has been changed to an asynchronous operation to prevent a minor delay in completing the enable process.
- The IntelSDK now attempts to connect to the `workspaceone.com` domain URLs instead of the previous `vmwservices.com` domain URLs. If the `workspaceone.com` domain URLs are accessible, IntelSDK will construct all its URLs using `workspaceone.com`.
   - Requirements for allowlisting the necessary URLs can be found here:  [Requirements](https://developer.apple.com/documentation/webkit/wknavigationdelegate/3223382-webview)
   - For more information on the domain migration, see: [Knowledge Base](https://kb.omnissa.com/s/article/6000882)

- The dSYM upload feature will no longer be supported by the servers after `Jun 30, 2025`
   - Please remove any build phases that were executing the script which uploaded dSYMs. After Jun 30, 2025, the upload attempt will fail with an error. 
      - Errors will be prefixed with: `Symbol File upload failed with result`
   - Additionally, delete the corresponding service account JSON file.
   - For more information, see our KB article: [End of Availability & Support for the App Crash Symbolication](https://kb.omnissa.com/s/article/6000838)

### Known Issues

none


## Omnissa IntelligenceSDK for iOS 25.1.2 Release - February 2025

### Minimum Requirements

- iOS 16.0 device or iPadOS 16.0 device
- tvOS devices and app extensions are no longer supported
- visionOS for Vision Pro devices is not supported

### New Features

none 

### Resolved Issues

- Fixes a potential deadlock.

### Known Issues

none


## Omnissa IntelligenceSDK for iOS 25.1.1 Release - May 28, 2025

### Minimum Requirements

- iOS 16.0 device or iPadOS 16.0 device
- tvOS devices and app extensions are no longer supported
- visionOS for Vision Pro devices is not supported

### New Features

none 

### Resolved Issues

- The IntelSDK now attempts to connect to the `workspaceone.com` domain URLs instead of the previous `vmwservices.com` domain URLs. If the `workspaceone.com` domain URLs are accessible, IntelSDK will construct all its URLs using `workspaceone.com`.
   - Requirements for allowlisting the necessary URLs can be found here:  [Requirements](https://developer.apple.com/documentation/webkit/wknavigationdelegate/3223382-webview)
- The DSYM upload feature will no longer be supported. For more information, refer to the knowledge base:
[End of Availability & Support for the App Crash Symbolication](https://kb.omnissa.com/s/article/6000838)
   - Please remove any build phases that were executing scripts to upload DSYMs.
   - Additionally, delete the corresponding service account JSON file.

### Known Issues

none


## Omnissa IntelligenceSDK for iOS 25.1.0 Release - February 2025

### Minimum Requirements

- iOS 16.0 device or iPadOS 16.0 device
- tvOS devices and app extensions are no longer supported
- visionOS for Vision Pro devices is not supported

### New Features

none 

### Resolved Issues

- A bug that was causing recursive loading of “about:blank“ pages in WKWebView under specific conditions was fixed.

### Known Issues

none


## Omnissa IntelligenceSDK for iOS 24.11.0 Release - December 2024

### Minimum Requirements

- iOS 15.0 device or iPadOS 15.0 device
- tvOS devices and app extensions are no longer supported
- visionOS for Vision Pro devices is not supported

### New Features

- Branding updates for migration to Omnissa.
- Protobuf dependency has been updated to the latest available stable release at the time of releasing IntelSDK - 28.3
- If DEX is enabled via the custom SDK settings, then a new event with the name “network_change” is sent to the Intelligence backend outside of the existing events. This event is triggered when there is a switch in SSID, cellular type, ethernet, or a disconnection. Disconnections are not reported if a new connection is made in less than 5 seconds.

### Resolved Issues

none

### Known Issues

none


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
	- For more information on this API, please visit 
		- [Telemetry Privacy Configuration](ios-privacy-config.md)
		- [setPrivacyConfiguration()](ws1intelligence.md#setprivacyconfigurationprivacyconfig-telemetryfeature)
		- [Enabling Telemetry Features in IntelligenceSDK for iOS](ios-enable-telemetry-features-in-intelsdk.md)

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

For more information, see: [Exporting Telemetry Data](ws1intelligence.md#exporting-telemetry-data)

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
