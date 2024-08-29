---
layout: page
title: Telemetry Feature Integration
hide:
  #- navigation
  - toc
---

## Export Data Collected by Telemetry Features

Users are now able to export data collected from the Telemetry Features they have Opted-Into. Use the following API to get Feature, Event, and Export formatted specific data in an Asynchronous fashion!

!!!Note
    This feature is available only if the app has opted-in to the DEX feature. See [Enabling DEX](crittercism.md#dex-telemetry-opt-in)

Telemetry Export API
```java
	
   /**
   * Interface for handling Telemetry Feature exported data.
   */
   interface TelemetryExportHandler {
      /**
      * Callback when data export has completed.
      * @param data String which will contain exported Telemetry Feature data,
      * null if error occurs.
      */
      fun onDataExport(data: String?)
   }
   /**
    * Asynchronously Export Data collected through enabled Telemetry Features.
    * @param feature Telemetry Feature to query data for.
    * @param eventType Type of Telemetry Event data to query for: Attributes, Events, or All.
    * @param exportType Format of the data exported, currently offering JSON support only.
    * @param dataHandler Interface called when data is ready to be exported.
    */
   public static void exportTelemetryFeatureData( @NonNull TelemetryFeature feature,
                                                  @NonNull TelemetryEventType eventType,
                                                  @NonNull TelemetryExportType exportType,
                                                  @NonNull TelemetryExportHandler dataHandler) 
```

Telemetry Export Example
```java

   // Implement the TelemetryExportHandler Interface to reach exported data asynchronously.
   val dataHandler = object:TelemetryExportHandler { 
    // Callback method for export. Data parameter will either have formatted Telemetry
    // data or be null if an issue occurs. 
    override fun onDataExport(data: String?) {
        Log.d(TAG, "Here is my exported data: ${data ?: "An issue has occurred."}")
      } 
   }
   
   Crittercism.exportTelemetryFeatureData(
            TelemetryFeature.All, // Query data from all enabled features.
            TelemetryEventType.ALL, // Returns values for all data types.
            TelemetryExportType.JSON, // Format to export data in.
            dataHandler) // Exported data handler callback.
```

