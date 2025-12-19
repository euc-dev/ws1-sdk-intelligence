---
layout: page
title: Intelligence SDK Allowed Apps
hide:
  #- navigation
  - toc
---

## Intelligence SDK Allowed Apps

The control configuration (UEM SDK Custom Settings) KVP `IntelSDKAllowedApps` (Allowed Apps lists) is a JSON Array which can be used to control which apps can report DEX Data.

- If the current app’s ID matches one of the IDs in the `IntelSDKAllowedApps` array, DEX data collection and transmission will be enabled for that app (assuming DEX is opted in). 
- If the current app’s ID is not in the list, DEX data collection and transmission will be blocked for that app, even if DEX is otherwise enabled. 
- If the `IntelSDKAllowedApps` key is missing or the array is empty, all apps will be allowed to transmit DEX data (default behavior).
- The SDK could dynamically start or stop DEX data transmission based on changes to this list when `setSDKControlConfig` is called.
    - If DEX is already opted in at the time the app has been allow listed, DEX data will start transmitting.
    - If DEX is opted out at the time the app has been allow listed, DEX will remain off until it is opted in.

Obtain your app ID from Omnissa Intelligence console. Please check [Omnissa Intelligence SDK Data for Apps](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelIntelligenceSDKApps.html) for more information.

## How to Use
To inject this control configuration, use the API `setSDKControlConfig` offered through the `Crittercism` singleton. The control configuration is the UEM SDK Custom Settings and can be fetched from the UEM SDK. For more details on how to fetch the Custom Settings, see [Workspace ONE UEM Custom Settings Integration for Intelligence SDK](../guides/custom-settings-integration.md). 


```Objective-C
/// Function to parse the entire custom settings JSON string payload for all entries related to IntelligenceSDK.
/// This function will process any the current and future SDK control configurations, so the host can simply pass the payload JSON string. (No need to process JSON into Dictionaries.)
/// The processing includes the DEXData JSON, so the `setPrivacyConfiguration` function is deprecated. The host app should not mix the use of both functions.
/// `setSDKControlConfig` will save the configuration data, so there is no need for the app to save the payload.
///     If `nil` or an empty string is specified, IntelligenceSDK will do nothing to the previously saved configuration.
+ (void)setSDKControlConfig:(nullable NSString *)config;
```

For more on this API, see: [setSDKControlConfig](ws1intelligence.md#setsdkcontrolconfigconfig).


### Control Configuration Attributes
The following is a table of the Attributes that are currently parsed within the control config.

#### Configuration JSON Attributes Table
|         Flag        |                                       Description                                       |   Value   |                                                   Outcome                                                   |                                      Notes                                     |
|:-------------------:|:---------------------------------------------------------------------------------------:|:---------:|:-----------------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------:|
| IntelSDKAllowedApps | JSON Array of Application IDs that are allowed to transmit DEX data to the UEM console. |           |                                                                                                             | This value of this key (JSON object) is parsed only if CaptureDEXData is true. |
|                     |                                                                                         |  MISSING  |                             All apps will transmit DEX data to the UEM console.                             |                                                                                |
|                     |                                                                                         | AVAILABLE | Only the matching Application IDs within the array will be allowed to transmit DEX data to the UEM console. |                                                                                |

### Example Payload

```JSON
  // Only the listed Android apps (App 1 and App 2) will be allowed to transmit DEX data.

  "IntelSDKAllowedApps": [
    "2b5a662e8c4d45fe9f8af99af509655b00555300",  // App 1 ID
    "84e3527ecb354764925d48345530c47f00555300"  // App 2 ID
  ]
```