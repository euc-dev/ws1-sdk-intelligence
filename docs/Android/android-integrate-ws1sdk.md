---
layout: page
title: Sending UEM Attributes To Intelligence SDK
hide:
  #- navigation
  - toc
---

A new Public API has been added to provide UEM attributes to the IntelligenceSDK’s DEX feature:

- This API takes an information provider interface which allows users to create getter methods for each available data attribute: **UEM Device UDID**, **UEM Serial Number**, **UEM Username**.

Android UEMInfo Interface
```JAVA
/**
* Interface for providing UEM Information to our DEX
*/
interface UemInfo {

/**
 * Returns the UEM Device UDID as a String
 */
fun getUemDeviceUDID(): String?

/**
 * Returns the UEM Serial Number as a String
 */
fun getUemSerialNumber(): String?

/**
 * Returns the UEM Username as a String
 */
fun getUemUsername(): String?
}
```

IntelligenceSDK UEMInfo Sample Implementation
```JAVA
import android.content.Context
import android.content.RestrictionsManager
import com.crittercism.uemprovider.UemInfo
import com.crittercism.uemprovider.UemInfoKeys

internal class IntelSDKUemInfo(context: Context): UemInfo {

        private val restrictionsMgr = context.getSystemService(Context.RESTRICTIONS_SERVICE) as RestrictionsManager

        override fun getUemDeviceUDID(): String = restrictionsMgr.applicationRestrictions.getString(UemInfoKeys.UEM_KEY_DEVICE_UDID,
                        UemInfoKeys.UEM_KEY_UNAVAILABLE)

        override fun getUemSerialNumber(): String = restrictionsMgr.applicationRestrictions.getString(UemInfoKeys.UEM_KEY_SERIAL_NUMBER,
                        UemInfoKeys.UEM_KEY_UNAVAILABLE)

        override fun getUemUsername(): String = restrictionsMgr.applicationRestrictions.getString(UemInfoKeys.UEM_KEY_USERNAME,
                        UemInfoKeys.UEM_KEY_UNAVAILABLE)
}
```

Registering IntelligenceSDK UEM Info

```JAVA
// Create instance of UemInfo
uemInfo = IntelSDKUemInfo(context)
Crittercism.setUemInfo(uemInfo)
Crittercism.initialize(context, APTELIGENT_APP_ID, config)
```

!!!Note
    To get UEM Information to DEX Feature, set UemInfo before enabling DEX through the Crittercism singleton. API to set UemInfo is Crittercism.setUemInfo(UemInfo info).

- If the app does not already have access to device-udid, serial-number, username and if it is managed, then it can be fetched from UEM to be injected into IntelligenceSDK.

- Within UEM, the required attributes can be set during app assignment within the ‘Application Configuration’ section as shown in the screenshot.

![](uem_application_configuration%20(1).png)

- When an app is pushed with the required attributes, the attributes can be queried and injected into IntelligenceSDK as shown in the code.
