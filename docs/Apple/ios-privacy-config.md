---
layout: page
title: Telemetry Privacy Configuration for iOS
hide:
  #- navigation
  - toc
---


The Privacy Configuration, or Config for short, is a JSON payload which can be used to control the reporting of Telemetry Feature Data.
This payload can either be generated manually or pulled from the WS1 SDK's Custom Settings if integrated in the application.

### How to Use

To inject this control configuration, use the API `setSDKControlConfig`. The control configuration is the UEM SDK Custom Settings and can be fetched from the UEM SDK. For more details on how to fetch the Custom Settings, see [Workspace ONE UEM Custom Settings Integration for Intelligence SDK](../guides/custom-settings-integration.md). 

```objective-c
/// Function to parse the entire custom settings JSON string payload for all entries related to IntelligenceSDK.
/// This function will process any the current and future SDK control configurations, so the host can simply pass the payload JSON string. (No need to process JSON into Dictionaries.)
/// The processing includes the DEXData JSON, so the `setPrivacyConfiguration` function is deprecated. The host app should not mix the use of both functions.
/// `setSDKControlConfig` will save the configuration data, so there is no need for the app to save the payload.
///     If `nil` or an empty string is specified, IntelligenceSDK will do nothing to the previously saved configuration.
+ (void)setSDKControlConfig:(nullable NSString *)config;
```

For more on this API, see: [setSDKControlConfig](ws1intelligence.md#setsdkcontrolconfigconfig).


### Privacy Configuration Attributes
The following is a table of the Attributes allowed within the Privacy Configuration JSON format.

#### Configuration JSON Attributes Table
|       **Flag**      | **Description**                                                                     |           **Value**          |                                              **Outcome**                                              |                                                                        **Notes**                                                                       |
|:-------------------:|-------------------------------------------------------------------------------------|:----------------------------:|:-----------------------------------------------------------------------------------------------------:|:------------------------------------------------------------------------------------------------------------------------------------------------------:|
|       DEXData       |                    JSON container for all DEX granular controls.                    |                              |                                                                                                       |                                     This value of this key (JSON object) is parsed only if CaptureDEXData is true.                                     |
|                     |                                                                                     |            MISSING           | All attributes within the DEX umbrella are reported.                                                  |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | Attributes are selectively reported as per the configured flags.                                      |                                                                                                                                                        |
|       Version       |  Helps version the JSON payload. Could be useful when introducing breaking changes. |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | The payload is considered un-usable.                                                                  |                                                                                                                                                        |
|                     |                                                                                     | AVAILABLE (But not a number) | The payload is considered un-usable.                                                                  |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | Payload is parsed accordingly                                                                         |                                                                                                                                                        |
|     BatteryData     |              JSON container for all `battery` entity granular controls.             |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All `battery` attributes are computed and reported.                                                   |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | `battery` attributes are selectively computed / reported as per the configured flags.                 |                                                                                                                                                        |
|      DeviceData     |              JSON container for all `device` entity granular controls.              |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All `device` attributes are computed and reported.                                                    |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | `device` attributes are selectively computed / reported as per the configured flags.                  |                                                                                                                                                        |
|     NetworkData     |              JSON container for all `network` entity granular controls.             |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All `network` attributes are computed and reported.                                                   |                                                                                                                                                        |
|                     |                                                                                     |           AVAILABLE          | `network` attributes are selectively computed / reported as per the configured flags.                 |                                                                                                                                                        |
|      DisableAll     | Used for enabling / disabling `battery`, `network` or `device` entities completely. |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All attributes are computed and reported for the corresponding entity.                                |                                                                                                                                                        |
|                     |                                                                                     |             TRUE             | All attributes are **not** computed and reported for the corresponding entity.                        |                                                                                                                                                        |
|                     |                                                                                     |             FALSE            | All attributes are computed and reported for the corresponding entity.                                |                                                                                                                                                        |
| AttributesToDisable |               Used for disabling specific attributes within an entity               |                              |                                                                                                       | - Non-Optional mandatory attributes cannot be disabled. They will be ignored if present in the array. - Optional mandatory attributes can be disabled. |
|                     |                                                                                     |            MISSING           | All attributes are computed and reported for the corresponding entity.                                |                                                                                                                                                        |
|                     |                                                                                     |      AVAILABLE AND EMPTY     | All attributes are computed and reported for the corresponding entity.                                |                                                                                                                                                        |
|                     |                                                                                     |    AVAILABLE AND NOT EMPTY   | Attributes in the array are disabled from being computed or transmitted for the corresponding entity. |                                                                                                                                                        |
|   EventsToDisable   |                 Used for disabling specific events within an entity                 |                              |                                                                                                       |                                                                                                                                                        |
|                     |                                                                                     |            MISSING           | All events are computed and reported for the corresponding entity.                                    |                                                                                                                                                        |
|                     |                                                                                     |      AVAILABLE AND EMPTY     | All events are computed and reported for the corresponding entity.                                    |                                                                                                                                                        |
|                     |                                                                                     |    AVAILABLE AND NOT EMPTY   | Events in the array are disabled from being computed or transmitted for the corresponding entity.     |                                                                                                                                                        |


### Examples

#### All enabled
To have all Telemetry Feature data enabled, no configuration payload needs to be injected through the SDK. All that is needed is to enable the Telemetry Feature itself.

However, if a payload needs to be specified it can be structured in the following way:
```JSON
{
  "DEXData": {
    "Version": 1.0
  }
}
```

#### Disable Data Categories
To disable an entire category of data event, set the "DisableAll" attribute to true  in the JSON payload for the specific Data category to disable.

Settings this attribute to false will continue to enable the data category.

```JSON
{
  "DEXData": {
    "Version": 1.0,
    "BatteryData": {
      "DisableAll": false
    },
    "DeviceData": {
      "DisableAll": true
    },
    "NetworkData": {
      "DisableAll": true
    }
  }
}
```
In the above example, we have disabled both `device` and `network` data categories entirely.

**Note:** Setting "DisableAll" to true will override any disabling of Attributes or Events for that category, as the entire category will be disabled.

#### Disable Specific Attributes
To disable specific attributes from being reported, please list them by Attribute name in their Specified Data Category under the JSON array element: "AttributesToDisable"

An empty list will not disable any attributes for the data category.

```JSON
{
  "DEXData": {
    "Version": 1.0,
    "BatteryData": {
      "AttributesToDisable": ["plugged_type"]
    },
    "DeviceData": {
      "AttributesToDisable": ["location_latitude", "location_longitude"]
    },
    "NetworkData": {
      "AttributesToDisable": []
    }
  } 
}
```

#### Disable Specific Events
To disable specific events from being reported, please list them by Event name in their Specified Data Category under the JSON array element: "EventsToDisable"

An empty list will not disable any events for the data category.

```JSON
{
  "DEXData": {
    "Version": 1.0,
    "BatteryData": {
      "EventsToDisable": ["charging_state_change"]
    },
    "DeviceData": {
      "EventsToDisable": []
    },
    "NetworkData": {
      "EventsToDisable": ["network_change"]
    }
  } 
}
```

#### Disable Specific Attributes and Events
This is an example combination of both Disabling Attributes and Disabling Events which can be read about in more detail above.

```JSON
{
  "DEXData": {
    "Version": 1.0,
    "BatteryData": {
      "AttributesToDisable": ["plugged_type"],
      "EventsToDisable": ["charging_state_change"]
    },
    "DeviceData": {
      "AttributesToDisable": ["location_latitude", "location_longitude"],
      "EventsToDisable": []
    },
    "NetworkData": {
      "AttributesToDisable": [],
      "EventsToDisable": ["network_change"]
    }
  } 
}
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


