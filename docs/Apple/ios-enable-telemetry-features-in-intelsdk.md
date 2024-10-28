---
layout: page
title: Enabling Telemetry Features in IntelligenceSDK for iOS
hide:
  #- navigation
  - toc
---

Updated October 28, 2024

# Telemetry Features


The two supported telemetry features that can be enabled from within IntelligenceSDK are - DEX and ZeroTrust. These features are disabled by default.

# Enabling DEX

Enabling DEX is a two step process - 

1. UEM admin must add an additional k-v pair named CaptureDEXData to the custom SDK settings payload.
1. Productivity apps must read the value of setting, and call API’s on the IntelligenceSDK to enable / disable the DEX feature. 

## Custom SDK Settings

### PolicyAllowCrashReporting

All productivity apps that have integrated with the WorkspaceONE UEM SDK can enable / disable DEX via custom SDK settings payload. Most of the apps if not all would already be using the following K-V pair in the custom SDK settings payload to enable / disable IntelligenceSDK - 

```JSON
{ 
    “PolicyAllowCrashReporting”: true
}
```

### CaptureDEXData

To enable DEX, a new key value pair named CaptureDEXData needs to be used as shown below - 

```JSON
{
    “PolicyAllowCrashReporting”: true,
    “CaptureDEXData”: true
}
```

- The value is a boolean, where true indicates that DEX reporting should be enabled and false indicates it must be disabled. 
- DEX reporting is turned off by default and is enabled when the required API to enable/disable is called explicitly. 


## IntelligenceSDK API’s to Toggle DEX / ZeroTrust

API’s on IntelligenceSDK can be called to toggle the DEX feature. DEX is disabled by default unless the following API’s across respective platforms are called.
 
### Opt-in / Opt-out API

```Objective-C

/*! Sets the Opt-In status for telemetry instrumentations, currently the options are Application, DEX,  ZeroTrust or AllAdvancedTelemetry.
 * -> Setting a telemetry instrumentation Opt-In to YES enables the collection of telemetry data.
 * -> The default for Application is YES. Enabling Application would start sending data to the Intelligence backend.
 * -> The default for DEX is NO. Enabling DEX would start sending data to the Intelligence backend.
 * -> The default for ZeroTrust is NO. Enabling ZeroTrust would `not` send data to the Intelligence backend.
 * -> If AllAdvancedTelemetry is used as a parameter then both `DEX` and `ZeroTrust` are enabled.
 * -> Disable features before enabling them again.
 *
 * @param type Application, DEX, ZeroTrust or AllAdvancedTelemetry
 * @param status new Opt-In status
 */
+ (void)setOptInStatusForType:(WS1TelemetryType)type andStatus:(BOOL)status;
/*! Returns the current Opt-In status for telemetry instrumentation type
 *  -> if `AllAdvancedTelemetry` is chosen, then this function returns true only if both
 *   `DEX` and `ZeroTrust` are enabled.
 *
 * @param type Application, DEX, ZeroTrust or AllAdvancedTelemetry.
 */
+ (BOOL)getOptInStatusForType:(WS1TelemetryType)type;
``` 

### WS1TelemetryType Enum


```Objective-C
typedef NS_ENUM(NSInteger, WS1TelemetryType) {
    WS1TelemetryTypeApplication = 0,
    WS1TelemetryTypeDEX,
    WS1TelemetryTypeZeroTrust,
    WS1TelemetryTypeAllAdvancedTelemetry
};
```
 
### Example of Enabling DEX / ZeroTrust

```Objective-C
The setTelemetryOptInStatus API could be used like below to enable DEX
[WS1Intelligence setOptInStatusForType:WS1TelemetryTypeDEX andStatus:YES];

// To opt out of DEX, API could be used like below - 
[WS1Intelligence setOptInStatusForType:WS1TelemetryTypeDEX andStatus:NO];
The setTelemetryOptInStatus API could be used like below to enable ZeroTrust
[WS1Intelligence setOptInStatusForType:WS1TelemetryTypeZeroTrust andStatus:YES];

// To opt out of ZeroTrust, API could be used like below - 
[WS1Intelligence setOptInStatusForType:WS1TelemetryTypeZeroTrust andStatus:NO];

// To opt-into all Advanced Telemetry (DEX and ZeroTrust), use the API like below - 
[WS1Intelligence setOptInStatusForType:WS1TelemetryTypeAllAdvancedTelemetry andStatus:YES];

// To opt-out of all Advanced Telemetry (DEX and ZeroTrust), use the API like below -
[WS1Intelligence setOptInStatusForType:WS1TelemetryTypeAllAdvancedTelemetry andStatus:NO];
```
 
### Example to Fetch DEX / ZeroTrust Enablement Status

```Objective-C
BOOL isDEXOptedIn = [WS1Intelligence getOptInStatusForType:WS1TelemetryTypeDEX];
BOOL isZeroTrustOptedIn = [WS1Intelligence getOptInStatusForType:WS1TelemetryTypeZeroTrust];
BOOL areAllAdvancedTelemetryFeaturesOptedIn = [WS1Intelligence getOptInStatusForType:WS1TelemetryTypeAllAdvancedTelemetry];
```

Important Notes when using the above API’s

- ** Always enable IntelSDK first i.e. start IntelSDK first before interacting with the API’s. Interacting with the API’s without enabling DEX will not have an impact on feature enablement. **
- ** Advanced Telemetry Features (DEX / ZeroTrust) is disabled by default. So if the custom settings do not exist or if it is disabled, please opt out of DEX using the above API. **
- When user opts-out of analytics from the privacy screen, its is application’s responsibility to opt-out of enabled advanced telemetry features as well.
    - Individual features cannot be opted out at this moment. Disabling one Advanced Telemetry feature (DEX / ZeroTrust) would turn off all other opted-in features. This is only applicable if more than one advanced telemetry feature is enabled by the application. 
 
