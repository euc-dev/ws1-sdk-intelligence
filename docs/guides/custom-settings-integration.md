---
layout: page
title: Workspace ONE UEM Custom Settings Integration for Intelligence SDK
hide:
  #- navigation
  - toc
---

# Adding Custom Settings into Workspace ONE UEM

## Navigate to the Custom Settings

To add custom settings into Workspace ONE UEM. follow these steps:
1. Go to: **Groups & Settings** > **All Settings** > **Apps** > **Settings & Policies** > **Settings**.
2. Enable **Custom Settings**.
3. Enter your desired custom settings in the **Custom Settings** field.
4. Save your changes.


## Structure of Custom Settings Using JSON
When entering custom settings, you can use **JSON format**. Make sure your JSON is properly structured and valid.

### Example JSON Custom Settings:

```json
{ 
	"PolicyAllowCrashReporting": true,
	"CaptureDEXData": true,
	"DEXData": {
		"Version": 1.0,
		"BatteryData": {
			"EventsToDisable": ["charging_state_change"]
		}
    }
    ...
}
```

## Common SDK Configuration Keys for Intelligence SDK
When configuring the Workspace ONE SDK through Custom Settings, specific key/value pairs can be used to for Intelligence SDK:

### `PolicyAllowCrashReporting`
**Type:** Boolean (`true` or `false`)

**Description:** Used to enable / disable Intelligence SDK

**Example:**
```JSON
{
  "PolicyAllowCrashReporting": true
}
```

### `CaptureDEXData`
**Type:** Boolean (`true` or `false`)

**Description:** Used to enable / disable the DEX portion of IntelSDK. This flag is void if `PolicyAllowCrashReporting` is not true.

**Example:**
```JSON
{
  "CaptureDEXData": false
}
```

### `DEXData`
**Type:** Object (JSON)

**Description:** Used for setting the privacy configuration JSON container for all DEX granular controls.

**Example:**
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
    },
    "AppUsageData": {
      "DisableAll": false
    }
  } 
}
```

### `IntelSDKAllowedApps`
**Type:** Object (JSON)

**Description:** A JSON array listing Application IDs allowed to transmit DEX data.

**Example:**
```JSON
{
  "IntelSDKAllowedApps": [
    "2b5a662e8c4d45fe9f8af99af509655b00555300",
    "84e3527ecb354764925d48345530c47f00555300"
  ]
}
```

## Integrating Custom Settings into Intelligence SDK using Workspace One SDK APIs
Once custom settings are configured in Workspace ONE UEM, your application can pull these settings at runtime.

In this example, the custom settings are retrieved from Workspace ONE UEM.

### Example (Android)
```kotlin
private fun fetchSettings() {
    SDKFetchSettingsHelper(context).fetchSDKSettings(context,
        object : ISdkFetchSettingsListener {
            override fun onSuccess(configuration: BaseConfiguration?) {
                val customSettings = configuration?.getValuesWithKeyStartWith(
                    SDKConfigurationKeys.TYPE_CUSTOM_SETTINGS,
                    SDKConfigurationKeys.CUSTOM_SETTINGS
                )
                ...
            }

            override fun onFailure(result: TaskResult?) {}
        }
    )
}
```

> For more details on setting the WS1 SDK Custom Settings, see: [setSDKControlConfig](../Android/crittercism.md#setsdkcontrolconfigconfig) 

### Example (iOS)
```C
- (void) receivedProfiles: (NSArray *) profiles {
    AWCustomPayload *custom = [[profiles firstObject] customPayload];
    NSString *customSettings = custom.settings;
    ...
}
```

For more details on setting the WS1 SDK Custom Settings, see: [setSDKControlConfig](ws1intelligence.md#setsdkcontrolconfigconfig)
	