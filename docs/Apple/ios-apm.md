---
layout: page
title: Network Insights
hide:
  #- navigation
  - toc
---

Whenever an app makes a network call, Omnissa Intelligence SDK monitors and captures certain information automatically. You can optionally configure filtering and location details. For an introduction, see [Network Insights Overview](../../dev-centre/ws1-intel/core-capabilities.md#network-insights-overview).

Omnissa Intelligence SDK ***automatically*** monitors the performance of network requests formed via the following APIs:

- all iOS versions
  - [NSURLConnection connectionWithRequest:delegate:]
  - [NSURLConnection sendAsynchronousRequest:queue:completionHandler]
  - [NSURLConnection sendSynchronousRequest:returningResponse:error:]
  - [NSURLConnection initWithRequest:delegate:]
  - [NSURLConnection initWithRequest:delegate:startImmediately:]
- iOS 8 and above
  - [NSURLSession dataTaskWithURL:]
  - [NSURLSession dataTaskWithURL:completionHandler:]
  - [NSURLSession dataTaskWithRequest:]
  - [NSURLSession dataTaskWithRequest:completionHandler:]
  - [NSURLSession downloadTaskWithURL:]
  - [NSURLSession downloadTaskWithURL:completionHandler:]
  - [NSURLSession downloadTaskWithRequest:]
  - [NSURLSession downloadTaskWithRequest:completionHandler:]
  - [NSURLSession downloadTaskWithResumeData:]
  - [NSURLSession downloadTaskWithResumeData:completionHandler:]

Omnissa Intelligence can also monitor requests made via web views. This form of monitoring must be explicitly enabled. See more details see [Monitoring Web Views](ws1config.md#monitoring-web-views).

- [WKWebView loadRequest:]

!!!Note
    Omnissa Intelligence does not monitor [UIWebView loadRequest:]. If you need this, consider using WKWebView or [Logging Network Request](ws1intelligence.md#logging-network-request).

## Disabling Network Insights

See [Network Insights Configuration](ws1config.md#network-insights-configuration) to disable automatic Network Insights.

## Filtering Captured Data

It is possible to customise filter blacklists that will completely discard matching URLs. See [CRFilter](crfilter.md) and [Network Insights Configuration](ws1config.md#network-insights-configuration) for more information.

## Configuring Location

Network Insights can tie location information to network data, if the application provides the location. By default, location information is not obtained by Omnissa Intelligence SDK.

You can update location information by using the method [updateLocation](ws1intelligence.md#updatelocationtolatitudelongitude).

## Further Reading

- [Logging Network Request](ws1intelligence.md#logging-network-request)
- [Network Insights Best Practices](https://www.apteligent.com/developer-resources/network-insights-best-practices/)
