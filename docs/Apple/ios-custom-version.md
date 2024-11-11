---
layout: page
title: Customizing the App Version Reported to Omnissa Intelligence Platform
hide:
  #- navigation
  - toc
---

Omnissa Intelligence Platform automatically groups all app performance data by app version. This allows monitoring for performance improvements and regressions each time a new app version is released.

By default, the Apple SDK reports the version as a combination of `CFBundleShortVersionString` and `CFBundleVersion` found in the app’s Info.plist file. If the `CFBundleShortVersionString` is not set, then only the CFBundleVersion will be reported. Typically both of these properties are set, and the version reported will look something like “2.0.1 (534)” (for example).

The default version that is reported is sufficient in most scenarios, but sometimes it is desirable to customize the app version that is reported to Omnissa Intelligence. A custom version may be specified by adding an additional property, CRAlternateVersion of type ***String*** to the app’s Info.plist file.

![](ios-cralternateversion-example.png)