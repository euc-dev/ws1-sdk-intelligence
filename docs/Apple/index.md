---
layout: page
title: Workspace ONE Intelligence SDK for iOS
hide:
  #- navigation
  - toc
---

## Introduction

This guide assumes you have an Workspace ONE Intelligence account set up with a valid app ID.

The Apple SDKs may be downloaded [here](../index.md#sdk-downloads).

## Release Notes

Release Notes can be viewed [here](release-notes.md).

## Application Requirements

- iOS 15.0 and above.
- SystemConfiguration Framework
- CoreData Framework

## Guides

- [Installation and Setup](install-ios.md)
- [Setup Automatic dSYM Uploads](install-ios.md#setup-automatic-dsym-uploads)
- [How to Customize App Version Reported to Workspace ONE Intelligence?](ios-custom-version.md)
- [SDK Data Sheet](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileIntelligenceSDK.html)

## SDK References

- [CRFilter](crfilter.md)
- [WS1Intelligence](ws1intelligence.md)
- [WS1Config](ws1config.md)
- [WS1Filter](ws1filter.md)
- [WS1Constants](ws1constants.md)

## Features

### Out-of-the-box Features

Some features are available as soon as you integrate Workspace ONE Intelligence SDK into your app.

- [Logging Crashes](ios-crash.md)
- [Network Insights](ios-apm.md)

### Main Features

- [Logging Breadcrumbs](ws1intelligence.md#logging-breadcrumbs)
- [Logging Errors and Handled Exceptions](ws1intelligence.md#logging-errors-and-handled-exceptions)
- [Logging User Flows](ws1intelligence.md#logging-user-flows)
- [Logging Network Request](ws1intelligence.md#logging-network-request)
- [Monitoring Web Views](ws1config.md#monitoring-web-views)
- [Enabling DEX](ws1config.md#dex-configuration)

### Other Features

- [Last Crash Notification](ws1constants.md#last-crash-notification)
- [Opt Out Status](ws1intelligence.md#opt-out-status)
- [Setting Log Verbosity Of Workspace ONE Intelligence SDK](ws1intelligence.md#setting-log-verbosity-of-workspace-one-intelligence-sdk)
- [Send Data On Wifi Only](ws1config.md#send-data-on-wifi-only)
- [Workspace ONE Intelligence SDK Device UUID](ws1intelligence.md#workspace-one-intelligence-sdk-device-uuid)
- [Sending UEM Attributes To Intelligence SDK](ios-integrate-ws1sdk.md)