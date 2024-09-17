---
layout: page
title: Crittercism
hide:
  #- navigation
  - toc
---

Import com.crittercism.app package to use this class.

## Initialization

### initialize (context, appID)

Initialize Workspace ONE Intelligence SDK. After calling this method, crashes and Network Insights information will be automatically reported to the Workspace ONE Intelligence servers. In addition, all other Workspace ONE Intelligence SDK methods will be enabled for use.

**Declaration**

```JAVA
public static synchronized void initialize (Context context, String appID)
```

**Parameters**

|   |   |
| --- | --- |
| context | An Android Context |
| appID | Your unique Workspace ONE Intelligence app ID which can be found on the Workspace ONE Intelligence web portal |

### initialize (context, appID, config)

Initialize Workspace ONE Intelligence SDK. After calling this method, crashes and Network Insights information will be automatically reported to the Workspace ONE Intelligence servers. In addition, all other Workspace ONE Intelligence SDK methods will be enabled for use.

**Declaration**

```JAVA
public static synchronized void initialize (Context context, String appID, CrittercismConfig config)
```

**Parameters**

|   |   |
| --- | --- |
| context | An Android Context |
| appID | Your unique Workspace ONE Intelligence app ID which can be found on the Workspace ONE Intelligence web portal |
| config | Instance of CrittercismConfig with the desired settings for Workspace ONE Intelligence SDK to use. See [CrittercismConfig](crittercism-config.md) for further information. |

## DEX Telemetry Opt-in

In order to use the DEX Telemetry Feature, the application must Opt-In using the following API after initializing the SDK.

Example: Set Opt In Status

```JAVA
// Must Initialize the Crittercism object before setting status
Crittercism.initialize(context, MY_APP_ID, crittercismConfig)

// Set and Get Application Opt Status
Crittercism.setOptInStatus(Crittercism.TelemetryType.APPLICATION, true) //Default for this Telemetry Type is true
val sdkOptInStatus = Crittercism.getOptInStatus(Crittercism.TelemetryType.APPLICATION)

// Set and Get DEX Feature Opt Status
Crittercism.setOptInStatus(Crittercism.TelemetryType.DEX, true) //Default for this Telemetry Type is false
val dexOptInStatus = Crittercism.getOptInStatus(Crittercism.TelemetryType.DEX)
```

## Logging Breadcrumbs

A ***breadcrumb*** is a developer-defined text string (up to 140 characters) that allows developers to capture app run-time information. Example breadcrumbs may include variable values, progress through the code, user actions, or low memory warnings. For an introduction, see [Breadcrumbs](../../dev-centre/ws1-intel/core-capabilities.md#breadcrumbs-overview).

### leaveBreadcrumb (breadcrumb)

Breadcrumbs provide the ability to track activity within your app. A breadcrumb is a free form string you supply, which will be timestamped, and stored in case a crash occurs. Crash reports will contain the breadcrumb trail from the run of your app that crashed, as well as that of the prior run.

Userflows will also contain breadcrumbs that are created between the userflow’s start and end timestamp.

Breadcrumbs are limited to 140 characters, and only the most recent 100 are kept.

Workspace ONE Intelligence SDK will automatically insert a breadcrumb marked session_start on each initial launch, or foreground of your app.

**Declaration**

```JAVA
public static void leaveBreadcrumb (String breadcrumb)
```

**Parameters**

|   |   |
| --- | --- |
| breadcrumb | The custom string to leave a breadcrumb, maximum of 140 characters |

### leaveUserflowSpecificBreadcrumb(final String userflowName, final String text)

Logs text that will be reported only to the specified userflow. Even if other userflows are open and active, they will not have this breadcrumb associated with it.

Breadcrumbs not specified under an active userflow will operate on the traditional breadcrumb logic.

Breadcrumbs are limited to 140 characters, and only the most recent 100 are kept.

**Declaration**

```JAVA
public static void leaveUserflowSpecificBreadcrumb(@NonNull final String userflowName, @NonNull final String text)
```

**Parameters**

|   |   |
| --- | --- |
| userflowName | Name of the userflow to assign breadcrumb with |
| text | The custom string to leave a breadcrumb, maximum of 140 characters |

Here’s an example of how to instrument a client:

Android Userflow Specific Breadcrumb Examples

```JAVA
Crittercism.leaveUserflowSpecificBreadcrumb("appLogin", "userTappedSubmit")
```

## Logging Handled Exceptions

### logHandledException (exception)

Handled exceptions may be used for tracking exceptions caught in a try/catch statement, 3rd party library exceptions, and monitoring areas in the code that may currently be using assertions. Handled exceptions can also be used to track error events such as low memory warnings. For an introduction, see [Handled Exceptions](../../dev-centre/ws1-intel/core-capabilities.md#handled-exceptions).

Handled exceptions are grouped by stacktrace, much like crash reports. Handled exceptions may be viewed in the “Handled Exceptions” area of the Workspace ONE Intelligence portal.

We limit logging handled exceptions to once per minute. If you’ve logged an exception within the last minute, we buffer the last five exceptions and send those after a minute has passed.

**Declaration**

```JAVA
public static void logHandledException (Throwable t)
```

**Parameters**

|   |   |
| --- | --- |
| exception | Exception to log |

See also [Introduction to Handled Exception](../../dev-centre/ws1-intel/core-capabilities.md#handled-exceptions).

## Logging Network Request

Network Insignts automatically monitors HTTP traffic generated by either java.net.HttpURLConnection or OkHttp on devices running Android Marshmallow and earlier versions.

If you are using OkHttp version 3.3.0 and above, please see [Instrumenting OkHttpClient (Beta)](#instrumenting-okhttpclient-beta).

For other Android versions or networking libraries, you can manually capture traffic data and provide it to Workspace ONE Intelligence SDK with the following methods:

### logNetworkRequest(method, url, latency, bytesRead, bytesSent, responseCode, error)

This method allows developers to manually log Network Insights data. This is useful for monitoring networking libraries that Workspace ONE Intelligence SDK does not itself instrument.

A network request will NOT be logged in the following cases:

- A null string is provided for the method parameter.
- A null URL is provided for the URL parameter.
- A negative value is provided for the response time parameter.
- An excessively large value is provided for the response time parameter (i.e., System.currentTimeMillis() - responseTimeMillis < 0).
- A negative value is provided for the bytes read parameter.
- A negative value is provided for the bytes sent parameter.

**Declaration**

```JAVA
public static void logNetworkRequest(String method,
                                     URL url,
                                     long latency,
                                     long bytesRead,
                                     long bytesSent,
                                     int responseCode,
                                     Exception error)
```

**Parameters**

|   |   |
| --- | --- |
| method | The connection method, such as GET, POST, HEAD, PUT, DELETE, TRACE, OPTIONS, CONNECT, PATCH |
| url | The endpoint URL |
| latency | The time between start of request and receipt of response, in milliseconds |
| bytesRead | The number of bytes included in response body |
| bytesSent | The number of bytes included in request body |
| responseCode | HTTP status code, generally 100-599, e.g. 200 == OK, 400 == Bad Request, can be 0 if there is an error |
| error | Exception instance for a failed request. Null if no error. |

This capability was introduced in Android SDK 5.1.0

See also:

- [Network Insights]()
- [Network Insights Best Practices](https://www.apteligent.com/developer-resources/network-insights-best-practices/)

### logNetworkRequest(method, urlString, latency, bytesRead, bytesSent, responseCode, error)

This method allows developers to manually log Network Insights data. This is useful for monitoring networking libraries that Workspace ONE Intelligence SDK does not itself instrument.

A network request will NOT be logged in the following cases:

A null string is provided for the method parameter.

A null URL is provided for the URL parameter.

A negative value is provided for the response time parameter.

An excessively large value is provided for the response time parameter (i.e., System.currentTimeMillis() - responseTimeMillis < 0).

A negative value is provided for the bytes read parameter.

A negative value is provided for the bytes sent parameter.

**Declaration**

```JAVA
public static void logNetworkRequest(String method,
                                     String urlString,
                                     long latency,
                                     long bytesRead,
                                     long bytesSent,
                                     int responseCode,
                                     Exception error)
```

method

The connection method, such as GET, POST, HEAD, PUT, DELETE, TRACE, OPTIONS, CONNECT, PATCH

urlString

The endpoint URL in String

latency

The time between start of request and receipt of response, in milliseconds

bytesRead

The number of bytes included in response body

bytesSent

The number of bytes included in request body

responseCode

HTTP status code, generally 100-599, e.g. 200 == OK, 400 == Bad Request, can be 0 if there is an error

error

Exception instance for a failed request. Null if no error.

Introduced in Android SDK 5.1.0

Network Insights

Network Insights Best Practices

### updateLocation (curLocation)

Inform Workspace ONE Intelligence SDK of the device’s most recent location for use with Network Insights.

If the app uses this method to provide a recent location to the SDK, then Network Insights will tie location information to the network data.

You have the option of updating location information with data given by android.location.LocationManager.NETWORK_PROVIDER or android.location.LocationManager.GPS_PROVIDER. This allows more accurate data to be sent to the server.

As explained in the Android Developer Guide, in order to receive location updates from the network or GPS provider, your `AndroidManifest.xml` file must include either the `ACCESS_COARSE_LOCATION` or `ACCESS_FINE_LOCATION` permissions.

**Declaration**

```JAVA
public static void updateLocation (Location curLocation)
```

**Parameters**

| --- | --- |
| location | Location of the user / device |

Introduced in Android SDK 4.2.0

## Logging User Metadata

Developers can set user metadata to tracking information about individual users. For an introduction, see [User Name](../../dev-centre/ws1-intel/core-capabilities.md#user-name-overview).

### setUsername (username)

Setting a username will allow the ability to monitor app performance for each user. We recommend setting a username to a value that can be tied back to your customer support system.

Valid inputs are strings with a length of 1 to 32 characters

**Declaration**

```JAVA
public static void setUsername (String username)
```

username

The new username

## Logging User Flows

User flows allow developers to track key interactions or user flows in their app such as login, account registration, and in app purchase.

For an introduction, see [User Flows Monitoring](../../dev-centre/ws1-intel/core-capabilities.md#user-flows-monitoring-overview).

### beginUserFlow (name)

Init and begin a user flow with a default value.

User flows allow developers to track key interactions or user flows in their app such as login, account registration, and in app purchase. The Workspace ONE Intelligence SDK will automatically track application load time as a user flow. You can specify additional user flows by adding code to your application.

Beginning a user flow starts a user flow. Ending, failing, or cancelling a user flow stops a user flow. Otherwise, the user flow will be marked as crashed or timed out. If a crash occurs, all in progress user flows are marked crashed and will be reported with the crash.

All user flows will appear on Workspace ONE Intelligence portal except for cancelled user flows.

User flows with the same name are aggregated together in the portal by Workspace ONE Intelligence. Only one user flow for a given name can be in progress at a given time. If you begin a second user flow with the same name while another is in progress, the first user flow will be cancelled and the new one will take its place.

**Declaration**

```JAVA
public static void beginUserFlow (String name)
```

name

The name of the user flow

see also

Introduction to User Flows

Three ways to improve mobile app experience with Workspace ONE Intelligence User Flows

Intelligence Use Case: Tracking User Experience through Mobile App Analytics

Leaving UserFlow-specific Breadcrumbs

### cancelUserFlow (name)

Cancel a user flow as if it never existed. The userflow will not be reported.

**Declaration**

```JAVA
public static void cancelUserFlow (String name)
```

name

The name of the user flow

### endUserFlow (name)

End an already begun user flow successfully.

**Declaration**

```JAVA
public static void endUserFlow (String name)
```

name

The name of the user flow

### failUserFlow (name, failureReason)

End an already begun user flow as a failure, and give a reason why it failed.

**Declaration**

```JAVA
public static void failUserFlow (String name)
```

name

The name of the user flow.

failureReason

The reason the user flow failed.

### failUserFlow (name)

End an already begun user flow as a failure.

**Declaration**

```JAVA
public static void failUserFlow (String name)
```

name

The name of the user flow

### setUserFlowValue (name, value)

Set the currency cents value of a user flow.

**Declaration**

```JAVA
public static void setUserFlowValue (String name, int value)
```

value

The value of the user flow

name

The name of the user flow

### getUserFlowValue (name)

Get the currency cents value of a user flow.

**Declaration**

```JAVA
public static int getUserFlowValue (String name)
```

name

The name of the user flow

## Detecting a Crash Occurred

### getPreviousSessionCrashData (crashListener)

Submit a callback to this method to get crash data from the previous session. The callback will receive crash information, see CrashData for details.

**Declaration**

```JAVA
public static void getPreviousSessionCrashData (CrittercismCallback<CrashData> crashListener)
```

CrittercismCallback

CrashData

### didCrashOnLastLoad ()

**Declaration**

```JAVA
public static boolean didCrashOnLastLoad ()
```

Returns True if the app crashed on the last load, returns False otherwise.

## Delay Sending App Load Data

### sendAppLoadData ()

Tell Workspace ONE Intelligence SDK to send an app load event that was delayed. By default, Workspace ONE Intelligence SDK sends app load events automatically when your app is started. However, if you set ..:ref:delaySendingAppLoad <delay_sending_app_load> flag to True on config, you can call this method to manually send app load events later.

**Declaration**

```JAVA
public static void sendAppLoadData ()
```

Delay App Load Configuration

## Instrumenting OkHttpClient (Beta)

Workspace ONE Intelligence Android SDK offers users the ability to instrument OkHttpClients to collect network insights.

### getNetworkInstrumentation()

Enables OkHttpClient instrumentation to collect network insights. It must be called on the main UI thread after the OkHttpClient is set. Once the method is invoked, Workspace ONE Intelligence SDK will automatically log network calls made with the returned instrumented client to the Network Insights page of the Workspace ONE Intelligence portal.

Here’s an example of how to instrument a client:

```JAVA
OkHttpClient uninstrumentedClient = new OkHttpClient();
OkHttpClient instrumentedClient = Crittercism.getNetworkInstrumentation().instrumentOkHttpClient(uninstrumentedClient);
// now you can use the instrumented client for network calls (on a different thread)
instrumentedClient.newCall(...).execute();
```

Introduced in Workspace ONE Intelligence Android SDK 5.8.11-beta11. Requires OkHttp 3.3.0 and above.

## Instrumenting WebView

Workspace ONE Intelligence Android SDK offers users the ability to instrument WebViews. This method only works for WebViews where JavaScript has already been enabled.

### instrumentWebView (webView)

Enables WebViews instrumentation. This method only works if JavaScript has already been enabled. It must be called on the UI thread after the WebViewClient is set and before the webpage is loaded. Once the method is invoked, Workspace ONE Intelligence SDK will automatically log any uncaught JavaScript exceptions to the Handled Exceptions page of the Workspace ONE Intelligence portal.

Here’s an example of how to configure and instrument a WebView:

```JAVA
// 1) Optionally set your own WebViewClient.
myWebView.setWebViewClient(myWebViewClient);

// 2) Enable JavaScript.
myWebView.getSettings().setJavaScriptEnabled(true);

// 3) Instrument WebView.
Crittercism.instrumentWebView(myWebView);
```

!!!Note
    Instrumenting WebView must be enabled in Hybrid Apps in order for API calls of Workspace ONE Intelligence SDK in the JavaScript code layer to communicate with the native code layer.

This capability was introduced in Android SDK 5.3.0

## Opt In Status

Workspace ONE Intelligence SDK provides an opt-in settings API that can 
enable or disable reporting for the Workspace ONE Intelligence features and 
can enable or disable reporting for the DEX feature. 
Each feature can be controlled individually. 
Note that each feature can have different default settings.

This allows developers to implement code that allows end users to decide 
which reporting features they want to enable for Workspace ONE Intelligence.

For an introduction, see Opt Out of Workspace ONE Intelligence.

### getOptInStatus (telemetryType)

Retrieve current opt-in status for the specified telemetry type.

**Declaration**

```JAVA
public static boolean getOptInStatus(final TelemetryType telemetryType)
```

Return True if the current opt-in state allows reporting Workspace ONE Intelligence SDK data for the specified telemetry type.

### setOptInStatus (telemetryType, isOptedIn)

If you wish to offer your users the ability to opt out of Workspace ONE Intelligence SDK’s reporting, you can set the OptInStatus for TelemetryType ‘Application’ to false. There will be no information/requests sent from that user’s app. If you do so, any pending crash reports will be purged.

If you wish to offer your users the ability to opt out of Workspace ONE Intelligence SDK’s DEX Telemetry reporting, you can set the OptInStatus for TelemetryType ‘DEX’ to false. No more device information will be sent from that user’s app.

Typically, a developer would connect these API calls to checkboxes in a settings menu.

**Declaration**

```JAVA
public static void setOptInStatus(final TelemetryType telemetryType, final boolean isOptedIn)
```

isOptedIn

set to False to disable Workspace ONE Intelligence SDK’s reporting for the specified TelemetryType

telemetryType

use Crittercism.TelemetryType.APPLICATION for crash, exceptions, userflows, breadcrumbs and network insights

use Crittercism.TelemetryType.DEX for device Digital Employee Experience information

## Setting Log Verbosity of Workspace ONE Intelligence SDK

By default, Workspace ONE Intelligence SDK prints a few messages to the device logcat that may be useful to verify that Workspace ONE Intelligence SDK is initialized and properly working. You can tune the amount of log messages that Workspace ONE Intelligence displays.

### getLoggingLevel ()

**Declaration**

```JAVA
public static LoggingLevel getLoggingLevel ()
```

The current logging level of the verbosity of log messages of Workspace ONE Intelligence SDK.

Logging Level Constants

## setLoggingLevel (loggingLevel)

Set the logging level to tune the verbosity of log messages of Workspace ONE Intelligence. The default value is Crittercism.LoggingLevel.Warning.

See Logging Level Constants

**Declaration**

```JAVA
public static void setLoggingLevel (LoggingLevel loggingLevel)
```

loggingLevel

The verbosity of Workspace ONE Intelligence SDK logging

Logging Level Constants

### Crittercism.LoggingLevel

Enum used to describe Workspace ONE Intelligence SDK logging level.

**Declaration**

```JAVA
public enum LoggingLevel {
    Silent,
    Error,
    Warning,
    Info,
    Debug }
```

**Constants**

|   |   |
| --- | --- |
| Crittercism.LoggingLevel.Silent | Turns off all log messages from Workspace ONE Intelligence SDK. |
| Crittercism.LoggingLevel.Error | Only print errors form Workspace ONE Intelligence SDK. An error is an unexpected event that will result not capturing important data. |
| Crittercism.LoggingLevel.Warning | Only print warnings from Workspace ONE Intelligence SDK. |
| Crittercism.LoggingLevel.Info | (Default) Print more verbose info logs. |
| Crittercism.LoggingLevel.Debug | The most verbose level of logging, used to print Workspace ONE Intelligence SDK debug logs. |

See also:

- [Setting Log Verbosity of Workspace ONE Intelligence SDK]()

## setPrivacyConfiguration(privacyConfig, telemetryFeature)
This API can be used to control reporting of certain Telemetry Feature Data such as attributes, events, or entire data collections through a Privacy Configuration map.

Ideally, this API will be used before enablement of Telemetry Features to ensure the all reports adhere to the specified Privacy Configuration.

**Declaration**
```JAVA
public static void setPrivacyConfiguration(Map<String, Object> privacyConfig, @NonNull TelemetryFeature feature)
```

**Parameters**

|               |   |
|---------------| --- |
| privacyConfig | Privacy Config Map containing the necessary key value pairs for parsing the privacy config. |
| feature       | The Feature classification of the Telemetry data to be exported, eg. DEX, ZeroTrust. |

See also:
- [Telemetry Privacy Configuration](privacy-config.md)