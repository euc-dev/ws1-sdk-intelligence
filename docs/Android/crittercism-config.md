---
layout: page
title: CrittercismConfig
hide:
  #- navigation
  - toc
---

Import com.crittercism.app package to use this class.

This object is used to specify various configuration options to Omnissa Intelligence SDK. Once this object is set up, you can pass it to `Crittercism.initialize()`. After Omnissa Intelligence SDK is initialized, changes to this object will have no effect.

## Creating a Config

### CrittercismConfig ()

Creates a new CrittercismConfig object with the default values for the above properties. You can modify the config values and pass this object into Crittercism.initialize().

**Declaration**

Java
```JAVA
public CrittercismConfig ()
```

Kotlin:
```Kotlin
public CrittercismConfig ()
```

Returns `CrittercismConfig` with default values.

**Examples**

Java Usage Example:
```JAVA
CrittercismConfig config = new CrittercismConfig();
Crittercism.initialize(context, appId, config)
```
Kotlin Usage Example:
```Kotlin
val config = CrittercismConfig()
Crittercism.initialize(context, appId, config)
```

### CrittercismConfig (configToCopy)

Create a copy of the input CrittercismConfig.

**Declaration**

```Java
public CrittercismConfig ((CrittercismConfig configToCopy)
```

**Parameters**

|   |   |
| --- | --- |
| configToCopy | CrittercismConfig to copy |

Returns a new instance of CrittercismConfig with the same values as configToCopy.

## Network Insights Configuration

### getURLDenylistPatterns ()

Retrieve a list of denylisted URLs. Network Insights data that pertains to URLs that match any of the patterns in the returned list will not be sent to Omnissa Intelligence. Patterns are case sensitive and may be any length.

**Declaration**

```Java
public List<String> getURLDenylistPatterns ()
```

Returns a copy of denylisted URLs for which Network Insights data will not be reported to Omnissa Intelligence.

### setURLDenylistPatterns (patterns)

Set a list of denylisted URLs. Network Insights data that pertains to URLs that match any of the patterns in the supplied list will not be sent to Omnissa Intelligence. Patterns are case sensitive and may be any length.

For example, you may provide URLs that may contain sensitive information. Note that by default all query parameters are removed before being sent to Omnissa Intelligence.

**Declaration**

```Java
public void setURLDenylistPatterns (List<String> patterns)
```

**Parameters**

|   |                                  |
| --- |----------------------------------|
| patterns | List of URL patterns to denylist |

### getPreserveQueryStringPatterns ()

Returns a copy of the list of URL string patterns for which the query strings will be preserved when Network Insights data is sent to Omnissa Intelligence. URLs that are not in this list will have their query strings removed before Network Insights data is sent to Omnissa Intelligence.

**Declaration**

```Java
public List<String> getPreserveQueryStringPatterns ()
```

Returns a copy of the list of URL string patterns for which the query strings will be preserved.

### setPreserveQueryStringPatterns (patterns)

Set a list of URL patterns for which the query strings will be preserved when Network Insights data is sent to Omnissa Intelligence. URLs that are not in this list will have their query strings removed before Network Insights data is sent to Omnissa Intelligence. If a URL in this list is also denylisted, then the denylist takes priority and network data will not be sent to Omnissa Intelligence for that URL.

Patterns are case sensitive and may be any length. A zero length string will result in all query strings being included in Network Insights data sent to Omnissa Intelligence.

**Declaration**

```Java
public void setPreserveQueryStringPatterns (List<String> patterns)
```

**Parameters**

|   |   |
| --- | --- |
| patterns | List of URL patterns for which to preserve the query strings |

## Set Custom Version Name

### getCustomVersionName ()

Retrieves the custom version name that will be reported to Omnissa Intelligence.

**Declaration**

```Java
public final String getCustomVersionName ()
```

Returns the custom version name that will be reported to Omnissa Intelligence, or null if Omnissa Intelligence SDK uses the android:versionName from the AndroidManifest.xml. .

### setCustomVersionName (customVersionName)

Set a custom app version that will be reported to Omnissa Intelligence.

By default, Omnissa Intelligence SDK uses the android:versionName from the AndroidManifest.xml when reporting data to Omnissa Intelligence. Depending on how teams version their apps, it may be desirable to set a custom version string.

**Declaration**

```Java
public final void setCustomVersionName (String customVersionName)
```

**Parameters**

|   |   |
| --- | --- |
| customVersionName | The version name that will be reported to Omnissa Intelligence. |

### isVersionCodeToBeIncludedInVersionString ()

Returns a boolean to indicate whether Omnissa Intelligence SDK concatenates the app version code to the app version that will be reported to Omnissa Intelligence. The app’s version code is defined in android:versionCode in the AndroidManifest.xml.

**Declaration**

```Java
public final boolean isVersionCodeToBeIncludedInVersionString ()
```

Returns True if the app’s version code is concatenated to the version name.

### setVersionCodeToBeIncludedInVersionString (shouldIncludeVersionCode)

Tells Omnissa Intelligence SDK whether or not to concatenate the app version code to the app version that will be reported to Omnissa Intelligence. The app’s version code is defined in android:versionCode in the AndroidManifest.xml.

**Declaration**

```Java
public final void setVersionCodeToBeIncludedInVersionString (boolean shouldIncludeVersionCode)
```

**Parameters**

|   |   |
| --- | --- |
| shouldIncludeVersionCode | True if the app’s version code should be concatenated to the version name. |

## Delay App Load Configuration

### delaySendingAppLoad ()

Determines if Omnissa Intelligence SDK delays automatic app load events until the developer calls sendAppLoadData.

By default, the return value is False and Omnissa Intelligence SDK automatically reports an app load event when Omnissa Intelligence SDK is initialized.

The delay app load feature is most often used to delay counting an app load until after a user has passed a login screen.

**Declaration**

```Java
public final boolean delaySendingAppLoad ()
```

Returns True if Omnissa Intelligence SDK should automatically send app load request. Returns False if the app will send app load request manually by calling sendAppLoadData.

### setDelaySendingAppLoad (delaySendingAppLoad)

Configure delayed app load events. By default, Omnissa Intelligence SDK immediately reports an app load when Omnissa Intelligence SDK is initialized. Set delaySendingAppLoad to True if the Omnissa Intelligence SDK should delay sending app load events until the app developer calls sendAppLoadData. Otherwise, set it to False to keep default behavior and send app loads automatically.

The delay app load feature is most often used to delay counting an app load until after a user has passed a login screen.

**Declaration**

```Java
public final void setDelaySendingAppLoad (boolean delaySendingAppLoad)
```

**Parameters**

|   |   |
| --- | --- |
| delaySendingAppLoad | A boolean to indicate whether Omnissa Intelligence SDK should delay automatic app loads. |

## Send Data On Wifi Only

### allowsCellularAccess ()

Returns a boolean to determine if Omnissa Intelligence SDK is allowed to send data while on a cellular network.

Default value is True.

public final boolean allowsCellularAccess ()
Returns True if Omnissa Intelligence SDK is allowed to send data while on a cellular network. Returns False if Omnissa Intelligence SDK is only sending data on Wifi network.

### setAllowsCellularAccess (allowsCellularAccess)

Set whether Omnissa Intelligence SDK sends data on cellular networks. The default value is True, which means that Omnissa Intelligence SDK sends data on both wifi and cellular networks. You can set it to False if Omnissa Intelligence SDK should only send data on wifi network.

This feature requires adding the ACCESS_NETWORK_STATE permission to your AndroidManifest.xml. Please see Permissions Required.

public final void setAllowsCellularAccess (boolean allowsCellularAccess)
allowsCellularAccess

Set to True to allow Omnissa Intelligence SDK to send data on a cellular network. Set to False to allow sending data on wifi only.

Introduced in Android SDK 5.7.0

## Include System Log Data (Logcat)

!!!Note
    Omnissa Intelligence platform does not support collection of system log data (logcat). The following no-op methods are left from old Apteligent platform and have no effect on the behavior of SDK versions 6.0.0 and above.

### isLogcatReportingEnabled ()

Returns a boolean to determine if system log data (logcat) is included in crashes reported by Omnissa Intelligence SDK to the old platform.

public final boolean isLogcatReportingEnabled ()
Returns True if system log data is included in crashes reported by Omnissa Intelligence SDK to the old platform.

### setLogcatReportingEnabled (shouldCollectLogcat)

Set whether system log data (logcat) should be included with crashes reported by Omnissa Intelligence SDK to the old Apteligent platform. Note that Omnissa Intelligence platform does not support logcat collection. Including system log data (logcat) can be helpful for debugging crashes and handled exceptions, but it comes at the cost of slightly increasing disk and network usage, which is why this feature must be manually enabled. Omnissa Intelligence SDK collects and sends the last 100 lines of logcat data to the old Apteligent platform.

public final void setLogcatReportingEnabled (boolean shouldCollectLogcat)
shouldCollectLogcat

Set to True to allow Omnissa Intelligence SDK to collect system log data. Set to False otherwise.

## Tenant Region Enabled

### setTenantRegionAllowed (isTenantRegionAllowed)
Set whether the client may send data to the Tenant Region, which may differ from developer's region, when a token is available. To enable the Tenant Region reporting, set to `true`.

Default value is `false`.

`public final void setTenantRegionAllowed(final boolean isTenantRegionAllowed)`


Tenant Region example in Java:
```JAVA
CrittercismConfig config = new CrittercismConfig();
config.setTenantRegionAllowed(true);
```

Tenant Region example in Kotlin:
```Kotlin
val config = CrittercismConfig()  
config.isTenantRegionAllowed = true
```

### isTenantRegionAllowed ()
Get whether Tenant Region reporting is allowed or not.

`public final boolean isTenantRegionAllowed()`

Tenant Region example in Java:
```JAVA
CrittercismConfig config = new CrittercismConfig();
config.setTenantRegionAllowed(true);
boolean tenantRegionAllowed = config.isTenantRegionAllowed();
```

Tenant Region example in Kotlin:
```Kotlin
val config = CrittercismConfig()  
config.isTenantRegionAllowed = true
val tenantRegionAllowed = config.isTenantRegionAllowed
```

## Caching Mode Enabled

### setCachingModeEnabled ()

In the Caching mode, the Intelligence SDK will cache events until the application is able to authenticate within in a 5 minute grace period. After 5 minutes, if we still haven’t authenticated, the Intel SDK will drop into the unauthenticated state and send event to the developer endpoint.

When Caching mode disabled, events will not wait for authentication and will instead send events to the current state endpoint. This means that if Caching mode disabled, and events are recorded while the SDK is trying to authenticate, they will be sent to the unauthenticated endpoint immediately. This can result in data not showing up on the in App Owner / Tenant Region Intelligence Consoles if not used correctly.

Default value is `true`.

`public final void setCachingModeEnabled(boolean cachingModeEnabled)`
Examples

Caching Mode example in Java
```JAVA
CrittercismConfig config = new CrittercismConfig();
config.setCachingModeEnabled(false);
```

Caching Mode example in Kotlin
```Kotlin
val config = CrittercismConfig()
config.isCachingModeEnabled = true
```

### isCachingModeEnabled ()

Returns a boolean to determine if caching mode is set.

`public final boolean isCachingModeEnabled()`
Examples

Caching Mode example in Java:
```JAVA
CrittercismConfig config = new CrittercismConfig();
config.setCachingModeEnabled(false);
boolean cachingModeEnabled = config.isCachingModeEnabled();
```
Caching Mode example in Kotlin
```Kotlin
val config = CrittercismConfig()
config.isCachingModeEnabled = true
val cachingModeEnabled = config.isCachingModeEnabled
```

## NDK Crash Enabled
NDK Crash Intelligence events will be reported to the Intelligence Console on the subsequent app load, after configuration and authentication if needed, following the crash
when enabled through the `CrittercismConfig`.
For more information on NDK Crash Events, refer to section: [NDK Crash](ndk-crash.md)

### isNdkCrashEnabled()
**Declaration**

Java
```JAVA
public boolean isNdkCrashEnabled()
```

Kotlin:
```Kotlin
fun isNdkCrashEnabled(): Boolean
```

Returns `Boolean` value representing if NDK / Native Crash Event reporting has been enabled.

### setNdkCrashEnabled(enabled)
**Declaration**

Java
```JAVA
public void setNdkCrashEnabled(boolean enabled)
```

Kotlin:
```Kotlin
fun isNdkCrashEnabled = enabled: Boolean
```

Set `Boolean` value to enable NDK / Native Crash Event reporting in the Intelligence SDK. The default value of this
enablement flag is `false`.