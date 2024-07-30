---
layout: page
title: Workspace ONE Intelligence SDK Constants
hide:
  #- navigation
  - toc
---

## Last Crash Notification

### WS1NotificationDidCrashOnLastLoad

Listen to this notification to get crash data from the previous session. The notification userInfo contains some useful information related to the crash.

The following example shows how you can register to receive crash notifications.

Objective-C
```C
NSNotificationCenter *center = [NSNotificationCenter defaultCenter];
NSOperationQueue *mainQueue = [NSOperationQueue mainQueue];
self.observer = [center addObserverForName:WS1NotificationDidCrashOnLastLoad
                                    object:nil
                                     queue:mainQueue
                                usingBlock:^(NSNotification *notification) {

        NSString *crashName = notification.userInfo[WS1NotificationCrashNameKey];
        NSString *crashReason = notification.userInfo[WS1NotificationCrashReasonKey];
        NSDate *crashDate = notification.userInfo[WS1NotificationCrashDateKey];
        // ...
    }];
```

Swift
```Swift
let center = NotificationCenter.default
let mainQueue = OperationQueue.main
let localChangeObserver = center.addObserver(forName: NSLocale.currentLocaleDidChangeNotification,
                                            object: nil,
                                            queue: mainQueue) { (notification) in

    let crashName = notification.userInfo?[WS1NotificationCrashNameKey];
    let crashReason = notification.userInfo?[WS1NotificationCrashReasonKey];
    let crashDate = notification.userInfo?[WS1NotificationCrashDateKey];
    // ...
 }
```

**Declaration**

Objective-C
```C
NSString * const WS1NotificationDidCrashOnLastLoad

```
Swift
```Swift
let WS1NotificationDidCrashOnLastLoad: String
```

### Last Crash UserInfo Keys

The [WS1NotificationDidCrashOnLastLoad](#ws1notificationdidcrashonlastload) userInfo contains the crash name, reason, and date.

**Constants**

|   |   |
| --- | --- |
| WS1NotificationCrashNameKey | Returns an NSString that describes the crash name |
| WS1NotificationCrashReasonKey | Returns an NSString that describes the crash reason |
| WS1NotificationCrashDateKey | Returns an NSString that informs the time of crash |

## WS1IntelligenceLoggingLevel

By default, Workspace ONE Intelligence SDK prints a few messages (via NSLog) to the device log that may be useful to verify that Workspace ONE Intelligence SDK is initialized and properly working. If you don’t want to see Workspace ONE Intelligence log messages, you can tune the amount of logging that is displayed.

See Setting Log Verbosity Of Workspace ONE Intelligence SDK of how to tweak the logging level

### WS1IntelligenceLoggingLevel

**Declaration**

Objective-C
```C
typedef enum : NSInteger {
   WS1IntelligenceLoggingLevelSilent,
   WS1IntelligenceLoggingLevelError,
   WS1IntelligenceLoggingLevelWarning,
   WS1IntelligenceLoggingLevelInfo,
   WS1IntelligenceLoggingLevelDebug
} WS1IntelligenceLoggingLevel;
```

Swift
```Swift
enum WS1IntelligenceLoggingLevel : Int {
    case Silent
    case Error
    case Warning
    case Info
    case Debug
}
```

**Constants**

|   |   |
| --- | --- |
| WS1IntelligenceLoggingLevelSilent | Turns off all Workspace ONE Intelligence SDK log messages |
| WS1IntelligenceLoggingLevelError | Only print errors. An error is an unexpected event that will result not capturing important data |
| WS1IntelligenceLoggingLevelWarning | ***(Default)*** Only print warnings. Currently warning messages are printed when calling Workspace ONE Intelligence SDK methods before initializing Workspace ONE Intelligence |
| WS1IntelligenceLoggingLevelInfo | The most verbose level of logging |
| WS1IntelligenceLoggingLevelDebug | Print internal logging in Workspace ONE Intelligence SDK. Please use this logging level and attach your log in your email to our support |

See also:

- [Setting Log Verbosity Of Workspace ONE Intelligence SDK](ws1intelligence.md#setting-log-verbosity-of-workspace-one-intelligence-sdk)