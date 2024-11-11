---
layout: page
title: WS1Filter
hide:
  #- navigation
  - toc
---

By default, Omnissa Intelligence SDK reports Network Insights data for all URLs that are accessed by your app and strips each URL of any query parameters. For example, http://foo.com?name=critter becomes http://foo.com. This is done to ensure personal information is not transmitted to Omnissa Intelligence.

It is also possible to customise filter deny list that will completely discard matching URLs. Network Insights data that pertains to any URL that matches the stored deny values will not be reported to Omnissa Intelligence.

Use the [WS1Filter filterWithString:] to modify the default filtering behavior:

**Declaration**

Objective-C
```C
// Add filters at initialization
WS1Config *config = [WS1Config defaultConfig];
config.urlFilters = @[[WS1Filter filterWithString:@"sensitiveURL"],
                      [WS1Filter filterWithString:@"additionalURL"]];
[WS1Intelligence enableWithAppID:@"MYAPPID" config:config];

// Add filters later on
[WS1Intelligence addFilter:[WS1Filter filterWithString:@"anotherURL"]];
```

Swift
```Swift

// Add filters at initialization
let config : WS1Config = WS1Config.default()
config.urlFilters = [WS1Filter(string: "sensitiveURL"), WS1Filter(string: "additionalURL")]
WS1Intelligence.enable(withAppID: appID, config: config)

// Add filters later on
WS1Intelligence.add(filter: WS1Filter)
```

!!!Note
    Filters match URLs via case sensitive substring matching

## Creating a Filter

### filterWithString:

Convenience method to create a deny filter.

**Declaration**

Objective-C
```C
+ (WS1Filter *)filterWithString:(NSString *)matchToken;
```

Swift
```Swift

class func filter(withString: matchToken)
```

**Parameters**

|   |   |
| --- | --- |
| matchToken | An NSString of the url to filter |

Returns a WS1Filter object for matchToken.

### filterWithString:andFilterType:

Convenience method to create a filter with the specified filter type.

**Declaration**

Objective-C
```C
+ (WS1Filter *)filterWithString:(NSString *)matchToken
                  andFilterType:(WS1FilterType)filterType
```

Swift
```Swift

class func filter(withString: matchToken andFilterType:filterType)
```

**Parameters**

|   |   |
| --- | --- |
| matchToken | An NSString of the url to filter |
| filterType | Indicates filter type |

Returns a WS1Filter object for matchToken.

### initWithString:

Initialize a filter that denies reporting for all URLs that match the specified token.

**Declaration**

Objective-C
```C
- (id)initWithString:(NSString *)matchToken;
```

Swift
```Swift

init?(string matchToken: String)
```

**Parameters**

|   |   |
| --- | --- |
| matchToken | An NSString of the url to filter |

Returns a WS1Filter object of type [WS1FilterTypeDeny](#ws1filtertype)

See also [WS1FilterType](#ws1filtertype)

### initWithString:andFilterType:

Initialize a filter that with the behavior specified by the filter type.

**Declaration**

Objective-C
```C
- (id)initWithString:(NSString *)matchToken
       andFilterType:(WS1FilterType)filterType;
```

Swift
```Swift

init?(string matchToken: String,
  filterType filterType: WS1FilterType)
```

**Parameters**

|   |   |
| --- | --- |
| matchToken | An NSString of the url to filter |
| filterType | Indicates filter type |

Returns a WS1Filter object with the indicated filter type.

See also [WS1FilterType](#constants)

## Other Methods

### doesMatch:

Returns YES if the filter matches the provided URL string

**Declaration**

Objective-C
```C
- (BOOL)doesMatch:(NSString *)url;
```

**Parameters**

|   |   |
| --- | --- |
| url | Specifies the url to be matched |

Returns YES when the filter matches the provided URL string

## Constants

### WS1FilterType

The filter type of WS1Filter

**Declaration**

Objective-C

```C
typedef enum : NSInteger {
   WS1FilterTypeDeny,
   WS1FilterTypePreserveQuery,
   WS1FilterTypePreserveFragment,
   WS1FilterTypePreserveParameters,
   WS1FilterTypePreserveAll
} WS1FilterType;
```

Swift
```Swift

enum WS1FilterType : Int {
    case Deny
    case PreserveQuery
    case PreserveFragment
    case PreserveParameters
    case PreserveAll
}
```

**Constants**

|   |   |
| --- | --- |
| WS1FilterTypeDeny | Deny reporting for certain endpoints. Request with a URL that matches this string are not reported |
| WS1FilterTypePreserveQuery | Prevent URL query string from being stripped out |
| WS1FilterTypePreserveFragment | Prevent URL fragment identifier from being stripped out |
| WS1FilterTypePreserveParameters | Prevent URL parameter string from being stripped out |
| WS1FilterTypePreserveAll | Prevent URL query, fragment, and parameters sections from being stripped out |
