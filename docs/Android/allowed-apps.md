---
layout: page
title: Intelligence SDK Allowed Apps
hide:
  #- navigation
  - toc
---

## Intelligence SDK Allowed Apps

The WS1 SDK Custom Settings KVP `IntelSDKAllowedApps` (Allowed Apps lists) is a JSON Array which can be used to control which apps can report DEX Data.
- If the current app’s ID matches one of the IDs in the `IntelSDKAllowedApps` array, DEX data collection and transmission will be enabled for that app (assuming DEX is opted in). 
- If the current app’s ID is not in the list, DEX data collection and transmission will be blocked for that app, even if DEX is otherwise enabled. 
- If the `IntelSDKAllowedApps` key is missing or the array is empty, all apps will be allowed to transmit DEX data (default behavior).
- The SDK could dynamically start or stop DEX data transmission based on changes to this list when `setSDKControlConfig` is called.
    - If DEX is already opted in at the time the app has been allow listed, DEX data will start transmitting.
    - If DEX is opted out at the time the app has been allow listed, DEX will remain off until it is opted in.

Obtain your app ID from Omnissa Intelligence console. Please check [Omnissa Intelligence SDK Data for Apps](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelIntelligenceSDKApps.html) for more information.

## How to Use
To inject this configuration, use the API `setSDKControlConfig` offered through the `Crittercism` singleton.

For more on this API, visit: [setSDKControlConfig](crittercism.md#setsdkcontrolconfigconfig)


```Java
/**
* Sets the WS1 UEM SDK Control Config.
* IntelligenceSDK will parse for the following properties:
*  - IntelSDKAllowedApps: JSON Array of apps allowed to transmit DEX data to the UEM console. If not available,
*    IntelligenceSDK will default to allowing all apps to transmit data.
*  - DEXData: JSON containing the privacy configuration.
*  Apps / SDK's are expected to fetch the control config JSON string and call this API. In the case the control
*  config is not available, a null value should be passed for the config parameter.
*
* @param config Json String of the control config provided from the WS1 UEM SDK.
*/
public static void setSDKControlConfig(String config)
```


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
    "CRITTERCISM_APP1_ID",  // App 1 ID
    "CRITTERCISM_APP2_ID"  // App 2 ID
  ]
```


### All Attributes and Events Reported By IntelligenceSDK
To Find all Attributes and Events reported by the Intelligence SDK, please refer to the following links based on platform and data type:

#### Android
- [Battery](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#battery_-_android)
- [Device](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#device_-_android)
- [Network](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#network_-_android)

#### iOS
- [Battery](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#battery_-_ios)
- [Device](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#device_-_ios)
- [Network](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileDeviceWide.html#network_-_ios)