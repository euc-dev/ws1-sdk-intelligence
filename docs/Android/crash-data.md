---
layout: page
title: CrashData
hide:
  #- navigation
  - toc
---

Import com.crittercism.app package to use this class.

CrashData provides information about crash that happens in the last session. See [Detecting a Crash Occurred](crittercism.md#detecting-a-crash-occurred) and [CrittercismCallback](crittercism-callback.md) for more details.

## Initialization

### CrashData (name, reason, timeOccured)

Creates a CrashData object with specified name, reason, and time of the crash

```JAVA
public CrashData (String name, String reason, Date timeOccurred)
```

|   |   |
| --- | --- |
| name | A String that describes the crash name |
| reason | A String that describes the crash reason |
| timeOccured | A Date that informs the time of crash |

## Crash Information

### getName ()

Returns a String object that describes the name of the crash.

**Declaration**

```JAVA
public String getName ()
```

### getReason ()

Returns a String object that describes why the crash happened.

**Declaration**

```JAVA
public String getReason ()
```

### getTimeOccurred ()

Returns a Date object that describes when the crash occured.

**Declaration**

```JAVA
public Date getTimeOccurred ()
```
