---
layout: page
title: Location In Intelligence SDK DEX Feature
hide:
  #- navigation
  - toc
---

## Location Data

IntelligenceSDK DEX feature can now request the location_longitude and location_latitude attributes for the device. This is true only if DEX is enabled.

The app must have the resources or entitlements needed to request location permission from the user, and the user must grant permission to use the device location. To prevent changing app behavior, IntelSDK will not trigger the request for location permission. 

### Apple Privacy Information

The IntelligenceSDK’s privacy manifest has been updated to show it collects precise location information.

App developers will need to include this location declaration in App Store submissions.

### Limitations

DEX native code will request a location update from iOS once every 5 minutes, when permissions allow.  Each location result is cached for a maximum of 5 minutes. 

### Background location

DEX stops requesting locations when the app is put into the background. 

DEX starts requesting locations again when the app returns to the foreground. 

DEX does not interact location updates portion of the background mode capability detailed in Handling location updates in the background 

