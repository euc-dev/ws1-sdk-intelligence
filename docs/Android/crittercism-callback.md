---
layout: page
title: CrittercismCallback
hide:
  #- navigation
  - toc
---

Import com.crittercism.app package to use this class.

This interface class is used to get crash data from the previous session. To use this class, you need to create a new class that implements the methods in this interface.

See [Detecting a Crash Occurred](crittercism.md#detecting-a-crash-occurred) for more details

## Interface Methods

### onDataReceived (t)

This method is called when Workspace ONE Intelligence SDK detects that there was a crash in the previous session.

This method will receive a CrashData. See [CrashData](crash-data.md) for details

**Declaration**

```JAVA
void onDataReceived(T t)
```

See also:

- [Detecting a Crash Occurred](crittercism.md#detecting-a-crash-occurred)
- [CrashData](crash-data.md)
