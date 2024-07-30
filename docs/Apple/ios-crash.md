---
layout: page
title: Logging Crashes
hide:
  #- navigation
  - toc
---

Once Workspace ONE Intelligence SDK is initialized within your app, crashes will automatically be captured and reported to the Workspace ONE Intelligence platform.

## Testing Crash Reporting

When testing crash reporting, be sure to disconnect the Xcode debugger. The debugger will hit a breakpoint before a crash actually occurs, which prevents Workspace ONE Intelligence SDK from detecting a crash. To detach the debugger, you can go to the ‘“Debug” menu in Xcode and click “Detach” while the app is running. Alternatively, you can click the “stop” button in the upper left hand corner of Xcode. Next, go to your device or simulator, and open the app manually. Now you should be able to test crash reporting without Xcode interfering.

## Handling Offline Crashes

If a user’s device does not have Internet connectivity, Workspace ONE Intelligence SDK caches any crashes that have occurred on the device until they can be sent to Workspace ONE Intelligence. By default, Workspace ONE Intelligence SDK caches up to three crashes on the device. If more than the maximum number of crashes occur, the oldest crash will be overwritten.

## Further Readings

- [Last Crash Notification](constants.md#last-crash-notification)