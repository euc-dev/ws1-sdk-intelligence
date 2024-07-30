---
layout: page
title: Installing the Android SDK
hide:
  #- navigation
  - toc
---

## Standard Integration¶

To integrate the Android Intelligence SDK into your application, you will need to pull the SDK artifacts from our Github Maven repository. This is a public repository, however currently Github credentials are still required to be provided.

```JAVA
// App level Build.Gradle Configuration

repositories {
    maven {
        name = "ws1-intelligencesdk-sdk-android"
        url = "https://maven.pkg.github.com/euc-releases/ws1-intelligencesdk-sdk-android/"
        // Here you will enter your Github Credentials
        credentials {
            username = "${GITHUB_USERNAME}"
            password = "$GITHUB_PERSONAL_ACCESS_TOKEN"
        }
    }
    mavenCentral()
    google()
}

android {
   packagingOptions {
        pickFirst '**/libc++_shared.so'
        pickFirst '**/libcrypto.1.0.2.so'
        pickFirst '**/libssl.1.0.2.so'
    }
}

def ws1IntelSdkVersion = "24.3.1"

dependencies {
    // Declare a dependency on the Intelligence SDK
    implementation "com.vmware.ws1:ws1intelligencesdk:$ws1IntelSdkVersion"
}
```

!!!Note
    For more information on using Github Maven Gradle Registry, please visit: [Working with the Gradle registry - GitHub Docs](https://docs.github.com/en/packages/working-with-a-github-packages-registry/working-with-the-gradle-registry)

## Integration Alongside Workspace ONE SDK

When integrating the Intelligence SDK alongside the Workspace ONE SDK, we must make sure to exclude some commonly packaged dependencies in order for seemly compilation

```JAVA
// App level Build.Gradle Configuration

repositories {
    maven {
        name = "ws1-intelligencesdk-sdk-android"
        url = "https://maven.pkg.github.com/euc-releases/ws1-intelligencesdk-sdk-android/"
        // Here you will enter your Github Credentials
        credentials {
            username = "${GITHUB_USERNAME}"
            password = "$GITHUB_PERSONAL_ACCESS_TOKEN"
        }
    }
    maven { url 'https://vmwaresaas.jfrog.io/artifactory/Workspace-ONE-Android-SDK/' }
    mavenCentral()
    google()
}

android {
   packagingOptions {
        pickFirst '**/libc++_shared.so'
        pickFirst '**/libcrypto.1.0.2.so'
        pickFirst '**/libssl.1.0.2.so'
    }
}

def ws1IntelSdkVersion = "24.3.1"
def ws1SdkVersion = "24.01"

dependencies {
    // Declare a dependency on the Intelligence SDK
    implementation "com.vmware.ws1:ws1intelligencesdk:$ws1IntelSdkVersion"
    // WS1 Dependency
    implementation("com.airwatch.android:AWFramework:$ws1SdkVersion"){
        // Need following excludes as they are duplicated in Workspace ONE SDK
        exclude group: 'com.airwatch.android', module: 'awApteligentBridge'
        exclude group: 'com.crittercism'
        exclude group: 'org.jetbrains.kotlinx', module: 'kotlinx-serialization-runtime'
    }
}
```

!!!Note
    Additional steps may be necessary to integrate the Workspace ONE SDK itself than what is shown above, please refer to Workspace ONE SDK specific integration documentation for that product.

## Permissions Required

Add the following permissions to your app’s `AndroidManifest.xml` file.

- INTERNET
  
  Required. Used to report data to Workspace ONE Intelligence.
- ACCESS_NETWORK_STATE
  
  Optional. Allows providing network connectivity information such as carrier and network type.

For more information on these permissions, refer to the [Android Manifest documentation](http://developer.android.com/reference/android/Manifest.permission.html).

## Permissions Needed for IntelligenceSDK Features

!!!Note
    These permissions are optional. If they are not given, the corresponding attributes will not be computed.

The IntelligenceSDK currently utilizes the following Android Permissions:

- ACCESS_FINE_LOCATION
- ACCESS_NETWORK_STATE
- ACCESS_WIFI_STATE
- BLUETOOTH
- BLUETOOTH_CONNECT
- CHANGE_NETWORK_STATE
- INTERNET
- READ_EXTERNAL_STORAGE
- READ_PHONE_STATE
- USE_BIOMETRIC
- WRITE_EXTERNAL_STORAGE

| Attribute | Permissions (Specific SDKs) |
| --- | --- |
| biometric_capabilities | USE_BIOMETRIC |
| biometric_credential_enrolled | USE_BIOMETRIC |
| bluetooth_connected | BLUETOOTH, BLUETOOTH_CONNECT (SDK 31+) |
| bssid | ACCESS_WIFI_STATE, ACCESS_FINE_LOCATION, ACCESS_COARSE_LOCATION, CHANGE_NETWORK_STATE (SDK 31+) |
| internet_available | ACCESS_NETWORK_STATE |
| ip_address | ACCESS_NETWORK_STATE |
| jitter | ACCESS_NETWORK_STATE |
| latency | ACCESS_NETWORK_STATE |
| mobile_network_type | READ_PHONE_STATE |
| network_connectivity_type | ACCESS_NETWORK_STATE |
| network_interface_change | ACCESS_NETWORK_STATE |
| ssid | ACCESS_WIFI_STATE, ACCESS_FINE_LOCATION, ACCESS_COARSE_LOCATION, CHANGE_NETWORK_STATE (SDK 31+) |
| vpn_connected | ACCESS_NETWORK_STATE |
| wifi_enablement | ACCESS_WIFI_STATE |
| wifi_frequency | ACCESS_WIFI_STATE (SDK < 31), ACCESS_FINE_LOCATION (SDK < 31), ACCESS_COARSE_LOCATION (SDK < 31), CHANGE_NETWORK_STATE (31+) |
| wifi_signal_strength | ACCESS_NETWORK_STATE, ACCESS_FINE_LOCATION(SDK < 30), ACCESS_COARSE_LOCATION(SDK < 30) |

For more information on Android Permissions, please refer to the following [Google Documentation](https://developer.android.com/guide/topics/permissions/overview/).

## Initializing Workspace ONE Intelligence SDK

The following initialization instructions are the same for both the standard (sdkonly) SDK and the NDK-enabled SDK.

Obtain your app ID from Workspace ONE Intelligence console.

1. To initialize Workspace ONE Intelligence SDK add the following code at the beginning of the onCreate() of your main Activity:

```JAVA
Crittercism.initialize(getApplicationContext(), "CRITTERCISM_APP_ID");
```

!!!Note
    Workspace ONE Intelligence SDK should be initialized once on the main thread as early as possible. Hence, if you subclass the Application singleton, you should initialize Workspace ONE Intelligence SDK in the onCreate() of that Application subclass using the same code, instead of the main activity. App loads will be sent from the first visible activity; they will not be sent from background services and BroadcastReceivers.

!!!Note
    Your main Activity is the one with android.intent.action.MAIN intent filter in your AndroidManifest.

Or initialize Workspace ONE Intelligence SDK with a CrittercismConfig argument

```JAVA
CrittercismConfig config = new CrittercismConfig();
config.setAllowsCellularAccess(false);
Crittercism.initialize(context, "CRITTERCISM_APP_ID", config);
```

2. You may need to add the following import, if Android Studio has not automatically added it already:

```JAVA
import com.crittercism.app.Crittercism;
```

Your Android app is now integrated with Workspace ONE Intelligence and you can go ahead and build it. Additional features require adding more code to your project.

## Configuring Proguard Symbolication

Symbolication is the process of translating stack traces into a human-readable form by mapping hexadecimal addresses to function names using symbol file(s). Workspace ONE Intelligence automatically symbolicates crashes once you have uploaded your app’s symbol file(s).

!!!Note
    Follow the instructions in this section only if your app obfuscates with [Android Proguard](http://developer.android.com/tools/help/proguard.html). Otherwise, skip this section.

For Android applications, developers have the option to obfuscate their function names using the ProGuard tool in order to reduce app size and to prevent others from reverse engineering the app source. In order to replace the obfuscated name with a human-readable name, developers use a Proguard mapping file.

### Setting Up ProGuard

1. Add the following lines to your project’s proguard.cfg file:

```JAVA
-dontwarn com.crittercism.**
-keep public class com.crittercism.**
-keepclassmembers public class com.crittercism.**
{
    *;
}
```

2. To get line number information, make sure that you keep the file names and line numbers in your ProGuard .cfg settings file.

```JAVA
-keepattributes SourceFile, LineNumberTable
```

This information will be visible in all stacktraces, however - even those that are not symbolicated.

To have your crashes automatically deobfuscated and grouped, you must upload a mapping.txt file on to the Settings tab of your App.

Each mapping.txt file you upload is associated with a version of your app. We deobfuscated crashes for only one version with each mapping.txt file.

!!!Note
    Be careful to upload the right file for your version!
    
    If you set a customized app version name in the CrittercismConfig instance, you should use that string and not the manifest string in app-version-name. Also, if you choose to include the app version code in the app version, that should also be included in app-version-name.

At this point, you have enabled Workspace ONE Intelligence SDK to receive Application Performance Information from your application
