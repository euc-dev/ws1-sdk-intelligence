---
layout: page
title: WS1Intelligence
hide:
  #- navigation
  - toc
---

## Initialization

### enable

Initializes Workspace ONE Intelligence SDK. This method will use the app ID string value specified in the application’s info.plist file with the key ‘WS1IntelligenceAppID’

**Declaration**

Objective-C
```C
+ (void)enable
```

Swift
```Swift
class func enable()
```

### enableWithConfig:

Initializes Workspace ONE Intelligence SDK with a config. This method will use the app ID string value specified in the application’s info.plist file with the key ‘WS1IntelligenceAppID’ After this call completes, changes to the config object will have no affect on the behavior of Workspace ONE Intelligence SDK.

**Declaration**

Objective-C
```C
+ (void)enableWithConfig:(WS1Config *)config
```

Swift
```Swift
class func enable(withConfig: config)
```

**Parameters**

|   |   |
| --- | --- |
| config | Your custom WS1Config |

### enableWithAppID:

Initializes Workspace ONE Intelligence SDK with the given App ID (found on the Workspace ONE Intelligence web portal).

**Declaration**

Objective-C
```C
+ (void)enableWithAppID:(NSString *)appId
class func enable(withAppID: appID)
```

**Parameters**

|   |   |
| --- | --- |
| appId | Your iOS appId |

## enableWithAppID:config:

Initializes Workspace ONE Intelligence SDK with the given App ID (found on the Workspace ONE Intelligence web portal). After this call completes, changes to the config object will have no affect on the behavior of Workspace ONE Intelligence SDK.

**Declaration**

Objective-C
```C
+ (void)enableWithAppID:(NSString *)appId config:(WS1Config *)config
```

Swift
```Swift
class func enable(withAppID: appID,
                     config: ws1Config)
```

**Parameters**

|   |   |
| --- | --- |
| appId | Your iOS appId |
| config | Your custom WS1Config |

## Logging Breadcrumbs

A ***breadcrumb*** is a developer-defined text string (up to 140 characters) that allows developers to capture app run-time information. Example breadcrumbs may include variable values, progress through the code, user actions, or low memory warnings. For an introduction, see [Breadcrums Overview](../../dev-centre/ws1-intel/core-capabilities.md#Breadcrumbs-overview).

### leaveBreadcrumb:

Breadcrumbs provide the ability to track activity within your app. A breadcrumb is a free form string you supply, which will be timestamped. Crash events, or other events, may refer to these for additional diagnostic details.

Breadcrumbs are limited to 140 characters.

**Declaration**

Objective-C
```C
+ (void)leaveBreadcrumb:(NSString *)breadcrumb
```

Swift
```Swift
class func leaveBreadcrumb(breadcrumb: String)
```

**Parameters**

|   |   |
| --- | --- |
| breadcrumb | The custom string to leave a breadcrumb, maximum of 140 characters |

### setAsyncBreadcrumbMode:

By default, breadcrumbs are flushed to disk immediately when written. This is by design - in order to provide an accurate record of everything that happened up until the point your app crashed. To improve performance you can instruct the library to perform all breadcrumb writes on a background thread. This means that breadcrums are not guaranteed to be saved immediately prior to a crash.

**Declaration**

Objective-C
```C
+ (void)setAsyncBreadcrumbMode:(BOOL)writeAsync
```

Swift
```Swift
class func setAsyncBreadcrumbMode(writeAsync: Bool)
```

**Parameters**

|   |   |
| --- | --- |
| writeAsync | YES to write breadcrumbs in a background thread |

See also [Breadcrums Overview](../../dev-centre/ws1-intel/core-capabilities.md#Breadcrumbs-overview).

## Logging Errors and Handled Exceptions

### logError:

Logging errors is a way of reporting NSError errors your app has received. If the method is passed an NSError, the stack trace of the thread that is logging the error will be displayed on the Crittercism web portal.

Logging errors may also be used for tracking NSError errors returned by Apple methods and 3rd party library errors. Errors are grouped by stacktrace, much like crash reports. Errors may be viewed in the “Handled Exceptions” area of the Workspace ONE Intelligence portal.

**Declaration**

Objective-C
```C
+ (BOOL)logError:(NSError *)error;
```

Swift
```Swift
class func logError(error: Error)
```

**Parameters**

|   |   |
| --- | --- |
| error | Error to log |

### logError:stacktrace:

Logging errors is a way of reporting NSError errors your app has received. If the method is passed an NSError, the stack trace of the thread that is logging the error will be displayed on the Crittercism web portal.

Logging errors may also be used for tracking NSError errors returned by Apple methods and 3rd party library errors. Errors are grouped by stacktrace, much like crash reports. Errors may be viewed in the “Handled Exceptions” area of the Workspace ONE Intelligence portal.

**Declaration**

Objective-C
```C
+ (BOOL)logError:(NSError *)error stacktrace:(NSArray *)stacktrace;
```

Swift
```Swift
class func logError(error: Error stacktrace: Stacktrace)
```

**Parameters**

|   |   |
| --- | --- |
| error | Error to log |
| stacktrace | Array of strings representing a stacktrace |

This capability was introduced in SDK v5.9.1

### logHandledException:

Handled exceptions may be used for tracking exceptions caught in a try/catch statement, 3rd party library exceptions, and monitoring areas in the code that may currently be using assertions. Handled exceptions can also be used to track error events such as low memory warnings. For an introduction, see Handled Exceptions.

Handled exceptions are grouped by stacktrace, much like crash reports. Handled exceptions may be viewed in the “Handled Exceptions” area of the Workspace ONE Intelligence portal.

**Declaration**

Objective-C
```C
+ (BOOL)logHandledException:(NSException *)exception
```

Swift
```Swift
class func logHandledException(exception: NSException)
```

**Parameters**

|   |   |
| --- | --- |
| exception | Exception to log |

See also Introduction to [Handled Exception](../../dev-centre/ws1-intel/core-capabilities.md#handled-exceptions).

## Logging Network Request

Network Insights data for NSURLSession and NSURLConnection is automatically captured and reported to Workspace ONE Intelligence simply by initializing the SDK. See [Network Insights](ios-apm.md).

If you want to log network requests manually or customize what data gets reported, use these methods.

### addFilter:

Adds an additional filter for network instrumentation.

**Declaration**

Objective-C
```C
+ (void)addFilter:(WS1Filter *)filter
```

Swift
```Swift
class func add(filter: WS1Filter)
```

**Parameters**

|   |   |
| --- | --- |
| filter | WS1Filter filter to add |

### logNetworkRequest:url:latency:bytesRead:bytesSent:responseCode:error:

Logging endpoints is a way of manually logging Network Insights data for custom network libraries which fall outside Workspace ONE Intelligence’s monitoring of NSURLConnection and NSURLSession method calls.

**Declaration**

Objective-C
```C
+ (BOOL)logNetworkRequest:(NSString *)method
                      url:(NSURL *)url
                  latency:(NSTimeInterval)latency
                bytesRead:(NSUInteger)bytesRead
                bytesSent:(NSUInteger)bytesSent
             responseCode:(NSInteger)responseCode
                    error:(NSError *)error
```

Swift
```Swift
class func logNetworkRequest(method: String,
                            url url: NSURL,
                    latency latency: NSTimeInterval,
                bytesRead bytesRead: Int,
                bytesSent bytesSent: Int,
          responseCode responseCode: Int,
                        error error: NSError) -> Bool
```

**Parameters**

|   |   |
| --- | --- |
| method | The connection method, such as GET, POST, HEAD, PUT, DELETE, TRACE, OPTIONS, CONNECT, PATCH |
| url | The endpoint URL |
| latency | The time between start of request and receipt of response, in seconds |
| bytesRead | The number of bytes included in response body |
| bytesSent | The number of bytes included in request body |
| responseCode | HTTP status code, generally 100-599, e.g. 200 == OK, 400 == Bad Request, can be 0 if there is an error |
| error | A non-nil error can be logged if network request failed to contact server, etc. Pass nil if there was no error |

Returns YES if the request was properly logged. Returns NO otherwise.

See also:

- [Network Insights](ios-apm.md)
- [Network Insights Best Practices](https://www.apteligent.com/developer-resources/network-insights-best-practices/)

### logNetworkRequest:urlString:latency:bytesRead:bytesSent:responseCode:error:

Logging endpoints is a way of manually logging Network Insights data for custom network libraries which fall outside Workspace ONE Intelligence’s monitoring of NSURLConnection and NSURLSession method calls.

**Declaration**

Objective-C
```C
+ (BOOL)logNetworkRequest:(NSString *)method
                urlString:(NSString *)urlString
                  latency:(NSTimeInterval)latency
                bytesRead:(NSUInteger)bytesRead
                bytesSent:(NSUInteger)bytesSent
             responseCode:(NSInteger)responseCode
                    error:(NSError *)error
```

Swift
```Swift
class func logNetworkRequest(method: String,
                urlString urlString: NSString,
                    latency latency: NSTimeInterval,
                bytesRead bytesRead: Int,
                bytesSent bytesSent: Int,
          responseCode responseCode: Int,
                        error error: NSError) -> Bool
```

**Parameters**

|   |   |
| --- | --- |
| method | The connection method, such as GET, POST, HEAD, PUT, DELETE, TRACE, OPTIONS, CONNECT, PATCH |
| urlstring | The endpoint URL in string |
| latency | The time between start of request and receipt of response, in seconds |
| bytesRead | The number of bytes included in response body |
| bytesSent | The number of bytes included in request body |
| responseCode | HTTP status code, generally 100-599, e.g. 200 == OK, 400 == Bad Request, can be 0 if there is an error |
| error | A non-nil error can be logged if network request failed to contact server, etc. Pass nil if there was no error |

Returns YES if the request was properly logged. Returns NO otherwise.

### updateLocationToLatitude:longitude:

Inform Workspace ONE Intelligence SDK of the device’s most recent location for use with event reporting.

**Declaration**

Objective-C
```C
+ (void)updateLocationToLatitude:(double)location longitude:(double)longitude
```

Swift
```Swift
class func updateLocation(toLatitude: double, longitude: double)
```

**Parameters**

|   |   |
| --- | --- |
| latitude | Location of the user / device |
| longitude | Location of the user / device |

This capability was introduced in SDK v5.9.3.

## Setting Username

This interface sets a relationship between a provided username string and the device UUID.

### setUsername:

**Declaration**

Objective-C
```C
+ (void)setUsername:(NSString *)username
```

Swift
```Swift
class func setUsername(username: String)
```

**Parameters**

|   |   |
| --- | --- |
| username | The new username |

## Logging User Flows

User flows allow developers to track key interactions or user flows in their app such as login, account registration, and in app purchase.

For an introduction, see [User Flows Monitoring](../../dev-centre/ws1-intel/core-capabilities.md#user-flows-monitoring-overview).

### beginUserFlow:

Inititializes and begins a user flow

User flows allow developers to track key interactions or user flows in their app such as login, account registration, and in app purchase. The Workspace ONE Intelligence SDK will automatically track application load time as a user flow. You can specify additional user flows by adding code to your application.

Beginning a user flow starts a user flow. Ending, failing, or cancelling a user flow stops a user flow. Otherwise, the user flow will be marked as crashed or timed out. If a crash occurs, all in progress user flows are marked crashed and will be reported with the crash.

All user flows will appear on Workspace ONE Intelligence portal except for cancelled user flows.

User flows with the same name are aggregated together in the portal by Workspace ONE Intelligence. Only one user flow for a given name can be in progress at a given time. If you begin a second user flow with the same name while another is in progress, the first user flow will be cancelled and the new one will take its place.

**Declaration**

Objective-C
```C
+ (void)beginUserFlow:(NSString *)name
```

Swift
```Swift
class func beginUserFlow(name: String)
```

**Parameters**

|   |   |
| --- | --- |
| name | The name of the user flow |

See also:

- [Introduction to User Flows](../../dev-centre/ws1-intel/core-capabilities.md#user-flows-monitoring-overview)
- [Three Ways to Improve User Experience through User Flows](https://www.apteligent.com/developer-resources/three-ways-to-improve-user-experience-through-userflows/)
- [Tracking User Experience with Advanced User Flows](https://www.apteligent.com/developer-resources/part-2-tracking-user-experience-with-advanced-userflows/)

### beginUserFlow:timeout:

Inititializes and begins a user flow with a specified timeout. If the user flow is not otherwise completed before this time elapses, the user flow will be marked as timed out.

User flows allow developers to track key interactions or user flows in their app such as login, account registration, and in app purchase. The Workspace ONE Intelligence SDK will automatically track application load time as a user flow. You can specify additional user flows by adding code to your application.

Beginning a user flow starts a user flow. Ending, failing, or cancelling a user flow stops a user flow. Otherwise, the user flow will be marked as crashed or timed out. If a crash occurs, all in progress user flows are marked crashed and will be reported with the crash.

All user flows will appear on Workspace ONE Intelligence portal except for cancelled user flows.

User flows with the same name are aggregated together in the portal by Workspace ONE Intelligence. Only one user flow for a given name can be in progress at a given time. If you begin a second user flow with the same name while another is in progress, the first user flow will be cancelled and the new one will take its place.

**Declaration**

Objective-C
```C
+ (void)beginUserFlow:(NSString *)name
              timeout:(NSTimeInterval)timeout
```

Swift

```Swift
class func beginUserFlow(name: String timeout: TimeInterval)
```

**Parameters**

|   |   |
| --- | --- |
| name | The name of the user flow |
| timeout | Timeout for this user flow in seconds |

See also:

- [Introduction to User Flows](../../dev-centre/ws1-intel/core-capabilities.md#user-flows-monitoring-overview)
- [Three Ways to Improve User Experience through User Flows](https://www.apteligent.com/developer-resources/three-ways-to-improve-user-experience-through-userflows/)
- [Tracking User Experience with Advanced User Flows](https://www.apteligent.com/developer-resources/part-2-tracking-user-experience-with-advanced-userflows/)

### cancelUserFlow:

Cancel a user flow as if it never existed. The user flow will not be reported.

**Declaration**

Objective-C
```C
+ (void)cancelUserFlow:(NSString *)name
```

Swift

```Swift
class func cancelUserFlow(name: String)
```

**Parameters**

|   |   |
| --- | --- |
| name | The name of the user flow |

### endUserFlow:

End an already begun user flow successfully.

**Declaration**

Objective-C
```C
+ (void)endUserFlow:(NSString *)name
```

Swift

```Swift
class func endUserFlow(name: String)
```

**Parameters**

|   |   |
| --- | --- |
| name | The name of the user flow |

### failUserFlow:

End an already begun user flow as a failure.

**Declaration**

Objective-C
```C
+ (void)failUserFlow:(NSString *)name
```

Swift

```Swift
class func failUserFlow(name: String)
```

**Parameters**

|   |   |
| --- | --- |
| name | The name of the user flow |

## Detecting a Crash Occurred

You can listen to `WS1NotificationDidCrashOnLastLoad` to get more app crash information on the previous run.

### didCrashOnLastLoad:

**Declaration**

Objective-C
```C
+ (BOOL)didCrashOnLastLoad
```

Swift

```Swift
class func didCrashOnLastLoad -> Bool
```

Returns YES if the app crashed on the last load, returns NO otherwise.

See also:

- [WS1NotificationDidCrashOnLastLoad](constants.md#last-crash-notification)
- [Last Crash UserInfo Keys](constants.md#last-crash-userinfo-keys)

## Opt Out Status

Workspace ONE Intelligence SDK provides an opt-out setting that disables all reporting to Workspace ONE Intelligence. This allows developers to implement code that asks end users whether they want to opt out of Workspace ONE Intelligence. For an introduction, see [Opt Out of Workspace ONE Intelligence](https://developer.omnissa.com/ws1-intel-dev-centre/hosting/overview/overview.html#opt-out).

### getOptOutStatus

Retrieve current opt out status.

**Declaration**

Objective-C
```C
+ (BOOL)getOptOutStatus
```

Swift

```Swift
class func getOptOutStatus -> Bool
```

Return YES if the user has opted out of reporting Workspace ONE Intelligence data.

### setOptOutStatus:

If you wish to offer your users the ability to opt out of Workspace ONE Intelligence data reporting, you can set the OptOutStatus to YES. If you do so, there will be no information/requests sent from that user’s app and any pending crash reports will be purged.

Typically, a developer would connect this API call to a checkbox in a settings menu.

**Declaration**

Objective-C
```C
+ (void)setOptOutStatus:(BOOL)status
```

Swift

```Swift
class func setOptOutStatus(status: Bool)
```

**Parameters**

|   |   |
| --- | --- |
| status | set to YES to disable Workspace ONE Intelligence |

## Setting Log Verbosity Of Workspace ONE Intelligence SDK

### loggingLevel

**Declaration**

Objective-C
```C
+ (WS1IntelligenceLoggingLevel)loggingLevel
```

Swift

```Swift
class func loggingLevel() -> WS1IntelligenceLoggingLevel
```

The current logging level of the verbosity of Workspace ONE Intelligence SDK log messages.

### setLoggingLevel:

Set the logging level to tune the verbosity log messages from Workspace ONE Intelligence SDK. The default value is WS1IntelligenceLoggingLevelWarning.

See [WS1IntelligenceLoggingLevel](constants.md#ws1intelligencelogginglevel) to see various logging levels.

**Declaration**

Objective-C
```C
+ (void)setLoggingLevel:(WS1IntelligenceLoggingLevel)loggingLevel
```

Swift

```Swift
class func setLoggingLevel(loggingLevel: WS1IntelligenceLoggingLevel)
```

**Parameters**

|   |   |
| --- | --- |
| loggingLevel | The log verbosity of Workspace ONE Intelligence SDK |

## Workspace ONE Intelligence SDK Device UUID

### getUserUUID

Get the unique identifier for this device generated by Workspace ONE Intelligence SDK. This is NOT the device’s UDID.

If called before enabling the SDK, this will return an empty string. All Workspace ONE Intelligence enabled apps on a device will share the UUID created by the first installed Workspace ONE Intelligence enabled app.

If all Workspace ONE Intelligence enabled applications are removed from a device, a new UUID will be generated when the next one is installed.

**Declaration**

Objective-C
```C
+ (NSString *)getUserUUID
```

Swift

```Swift
class func getUserUUID -> String
```
