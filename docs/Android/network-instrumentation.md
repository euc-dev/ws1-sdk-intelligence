---
layout: page
title: NetworkInstrumentation
hide:
  #- navigation
  - toc
---

Import com.crittercism.app package to use this class.

## Instrumenting OkHttpClient (Beta)

Workspace ONE Intelligence Android SDK offers users the ability to instrument OkHttpClients to collect Network Insights data.

### instrumentOkHttpClient(okHttpClient)

Enables OkHttpClient instrumentation to collect network insights. It must be called on the main UI thread after the OkHttpClient is set. Once the method is invoked, Workspace ONE Intelligence SDK will automatically log network calls made with the returned instrumented client to the Network Insights page of the Workspace ONE Intelligence portal.

Here’s an example of how to instrument a client:

```JAVA
OkHttpClient uninstrumentedClient = new OkHttpClient();
OkHttpClient instrumentedClient = Crittercism.getNetworkInstrumentation().instrumentOkHttpClient(uninstrumentedClient);
// now you can use the instrumented client for network calls (on a different thread)
instrumentedClient.newCall(...).execute();
```

Introduced in Workspace ONE Intelligence Android SDK 5.8.11-beta11. Requires OkHttp 3.3.0 and above.