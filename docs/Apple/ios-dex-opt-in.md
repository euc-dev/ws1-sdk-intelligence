---
layout: page
title: Opting into DEX
hide:
  #- navigation
  - toc
---

# Opting into DEX

Users are now able to individually opt in for Application or DEX telemetry data collection.

```objective-c

   typedef NS_ENUM(NSInteger, WS1TelemetryType) {
       WS1TelemetryTypeApplication = 0,
       WS1TelemetryTypeDEX,
   };
   
   /*! Sets the Opt-In status for telemetry instrumentations, currently the two
    * options are Application or DEX. Setting a telemetry instrumentation Opt-In to YES enables the
    * collection of telemetry data.
    * The default for Application is YES
    * The default for DEX is NO
    * @param type Application or DEX
    * @param status new Opt-In status
    */
   + (void)setOptInStatusForType:(WS1TelemetryType)type andStatus:(BOOL)status;
```

```objective-c

   /*! Returns the current Opt-In status for telemetry instrumentation type
    * @param type Application or DEX
    */
   + (BOOL)getOptInStatusForType:(WS1TelemetryType)type;
```

**Important Notes when using the above API’s**

- **Always enable IntelSDK first** i.e. start IntelSDK first before interacting with the APIs. 

   Interacting with the API’s without enabling DEX will not have an impact on feature enablement.

- **DEX is disabled by default.** 

   So if the custom settings do not exist or if it is disabled, please out out of DEX using the above API.



