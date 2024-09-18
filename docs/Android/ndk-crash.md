---
layout: page
title: NDK Crash
hide:
  #- navigation
  - toc
---

## NDK Crash Reporting

NDK Crash events will be reported to the Intelligence Console on the subsequent app load, after configuration and authentication if needed, following the crash.

These NDK Crash events can be found in the Crash section of the Intelligence Console, and can isolated by table filters such as setting `is_ndk` to true.

## NDK Crash Information
General Intelligence Crash Event attributes and definitions can be found here: [Crash - Android](https://docs.omnissa.com/bundle/WS1Intelligence/page/IntelExpMngtDefMobileIntelligenceSDK.html#crashes_-_android)
Key information behind the Native Crash caught will be sent as the `reason` attribute for the Crash Event.

This reason will be structured as a JSON block, containing dump information around the crash.

An example of the NDK crash info appears as such:
### “MyException” Custom C++ Exception Example:
```JSON
{
  "name": "11MyException",
  "exception_message": "This is a really important crash message!",
  "signal_number": 6,
  "signal_error_number": 0,
  "signal_code": -1,
  "signal_value": 0,
  "sending_process_id": 865,
  "sending_process_real_user_id": 10240,
  "faulting_instruction_address": 43980465111905,
  "exit_status": 0,
  "sigpoll_band": 43980465111905,
  "timer_id": 865,
  "overrun_count": 10240
}
```

### SEGFAULT Example:
```JSON
{
  "name":"SIGSEGV",
  "exception_message":"",
  "signal_number":11,
  "signal_error_number":0,
  "signal_code":-6,
  "signal_value":0,
  "sending_process_id":5047,
  "sending_process_real_user_id":10243,
  "faulting_instruction_address":43993350017975,
  "exit_status":0,
  "sigpoll_band":43993350017975,
  "timer_id":5047,
  "overrun_count":10243
}
```

### Json Information Attributes
- `name`
  - `String` - Demangled name of exception if possible, mangled if not. If Signal crash, “C-style crash”, the type of signal will appear here. Ex. “SIGBUS”
  
- `exception_message` 
  - `String`- Message of the exception if provided, else empty string
  
- `signal_number`
  - `Number` - type of signal captured. 
    - `SIGILL` - 4 
      - This signal denotes illegal instruction. 
    - `SIGTRAP` - 5 
      - This signal is sent to process when an exception has occurred. 
    - `SIGABRT` - 6 
      - If an error itself is detected by the program then this signal is generated using call to abort(). This signal is also used by the standard library to report an internal error. assert() function in c++ also uses abort() to generate this signal. 
    - `SIGBUS` - 7 
      - This signal is produced when an invalid memory is accessed. 
    - `SIGFPE` - 8 
      - This error signal denotes some arithmetic error or floating-point error. 
    - `SIGSEGV` - 11 
      - The signal is generated when a process tries to access a memory location not allocated to it.
    - `SIGSYS` - 31 
      - This signal is sent to process when an invalid argument is passed to a system call.

- `signal_error_number`
  - `Number` - error number defined in the file <errno.h>. 
    - Numbers can be found in the following file reference: [errno.h](https://android.googlesource.com/kernel/lk/+/dima/for-travis/include/errno.h)

- `signal_code`
  - `Number` - the cause of the signal. 
    - Code numbers can found in the following file reference: [signal.h](https://github.com/openbsd/src/blob/master/sys/sys/signal.h)

- `signal_value`
  - `Number` - value passed to signal if applicable.

- `sending_process_id`
  - `Number` - process ID which sent the signal.

- `sending_process_real_user_id`
  - `Number` - the “real user ID” of the process which sent the signal.

- `faulting_instruction_address`
  - `Number` - address of the faulting instruction.

- `exit_status`
  - `Number` - exit value of the signal

- `sigpoll_band`
  - `Number` - band event for SIGPOLL if relevant.

- `timer_id`
  - `Number` - POSIX timer ID.

- `overrun_count`
  - `Number` - POSIX timer overrun count.

