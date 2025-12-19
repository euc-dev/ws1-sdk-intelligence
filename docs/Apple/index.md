---
layout: page
title: Omnissa Intelligence SDK for iOS
hide:
  #- navigation
  - toc
---

## Introduction

This guide assumes you have an Omnissa Intelligence account set up with a valid app ID.

## Release Notes

Release Notes can be viewed [here](release-notes.md).

## Application Requirements

- Devices running iOS 16.0 or iPadOS 16.0 or newer.
- WS1SDK version 25.04.1 or newer is required for the two to interact. 
	- Workspace ONE UEM Console 2402 or later
- SystemConfiguration Framework
- CoreData Framework

- not supported:

    - tvOS devices
    - app extensions
    - visionOS for Vision Pro devices


## Guides

- [Installation and Setup](install-ios.md)
- [How to Customize App Version Reported to Omnissa Intelligence](ios-custom-version.md)
- [SDK Data Sheet](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileIntelligenceSDK.html)

## SDK References

- [CRFilter](crfilter.md)
- [WS1Intelligence](ws1intelligence.md)
- [WS1Config](ws1config.md)
- [WS1Filter](ws1filter.md)
- [WS1Constants](ws1constants.md)

## Features

### Out-of-the-box Features

Some features are available as soon as you integrate Omnissa Intelligence SDK into your app.

- [Logging Crashes](ios-crash.md)
- [Network Insights](ios-apm.md)

### Main Features

- [Logging Breadcrumbs](ws1intelligence.md#logging-breadcrumbs)
- [Logging Errors and Handled Exceptions](ws1intelligence.md#logging-errors-and-handled-exceptions)
- [Logging User Flows](ws1intelligence.md#logging-user-flows)
- [Logging Network Request](ws1intelligence.md#logging-network-request)
- [Monitoring Web Views](ws1config.md#monitoring-web-views)
- [Enabling DEX](ws1config.md#dex-configuration)
- [Opting into DEX](ios-dex-opt-in.md#opting-into-dex)

### Other Features

- [Last Crash Notification](ws1constants.md#last-crash-notification)
- [Opt Out Status](ws1intelligence.md#opt-out-status)
- [Setting Log Verbosity Of Omnissa Intelligence SDK](ws1intelligence.md#setting-log-verbosity-of-workspace-one-intelligence-sdk)
- [Send Data On Wifi Only](ws1config.md#send-data-on-wifi-only)
- [Omnissa Intelligence SDK Device UUID](ws1intelligence.md#workspace-one-intelligence-sdk-device-uuid)
- [Sending UEM Attributes To Intelligence SDK](ios-integrate-ws1sdk.md)
- [Location In Intelligence SDK DEX Feature](ios-dex-location.md)
