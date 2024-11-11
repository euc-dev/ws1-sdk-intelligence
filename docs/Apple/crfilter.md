---
layout: page
title: CRFilter
hide:
  #- navigation
  - toc
---

By default, Omnissa Intelligence SDK reports Network Insights data for all URLs that are accessed by your app and strips each URL of any query parameters. For example, http://foo.com?name=critter becomes http://foo.com. This is done to ensure personal information is not transmitted to Omnissa Intelligence.

It is also possible to customise filter denylists that will completely discard matching URLs. Network Insights data that pertains to any URL that matches the deny URL will not be reported to Omnissa Intelligence.

Use the [CRFilter filterWithString:] to modify the default filtering behavior:

**Declaration**

Objective-C
```C
// Add filters at initialization
CrittercismConfig *config = [CrittercismConfig defaultConfig];
config.urlFilters = @[[CRFilter filterWithString:@"sensitiveURL"],
                      [CRFilter filterWithString:@"additionalURL"]];
[Crittercism enableWithAppID:@"MYAPPID" andConfig:config];

// Add filters later on
[Crittercism addFilter:[CRFilter filterWithString:@"anotherURL"]];
```

Swift
```Swift
// Add filters at initialization
let config : CrittercismConfig = CrittercismConfig.default()
config.urlFilters = [CRFilter(string: "sensitiveURL"), CRFilter(string: "additionalURL")]
Crittercism.enable(withAppID: appID, and config: config)

// Add filters later on
Crittercism.add(filter: CRFilter)
```

!!!Note
    - Filters match URLs via case sensitive substring matching
    - Older versions of the agent did not scrub query parameters by default, and so a `queryOnlyFilter` existed. To avoid compile time errors for existing Omnissa Intelligence users, this filter type can still be created, but it will be ignored by the library. This filter type is deprecated, and will be removed in a future version of the agent.

## Creating a Filter

### filterWithString:

Convenience method to create a deny filter.

**Declaration**

Objective-C
```C
+ (CRFilter *)filterWithString:(NSString *)matchToken;
```

Swift
```Swift
class func filter(withString: matchToken)
```

**Parameters**

|   |   |
| --- | --- |
| matchToken | An NSString of the url to filter |

Returns a CRFilter object for matchToken.

### ~~queryPreservingFilterWithString:~~

Convenience method to create a filter which preserves the query portion of.

**Declaration**

Objective-C
```C
+ (CRFilter *)queryPreservingFilterWithString:(NSString *)matchToken
```

Swift
```Swift
class func queryPreservingFilterWithString(matchToken: String)
```

**Parameters**

|   |   |
| --- | --- |
| matchToken | An NSString of the url to filter |

### initWithString:

Initialize a filter that denies reporting of all URLs that match the specified token.

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

Returns a CRFilter object of type CRFilterTypeDeny

See also [CRFilterType](#crfiltertype)

- initWithString:andFilterType:

Initialize a filter that denies reporting for all URLs that match the specified token.

**Declaration**

Objective-C
```C
- (id)initWithString:(NSString *)matchToken
       andFilterType:(CRFilterType)filterType;
```

Swift
```Swift
init?(string matchToken: String,
  filterType filterType: CRFilterType)
```

**Parameters**

|   |   |
| --- | --- |
| matchToken | An NSString of the url to filter |
| filterType | Indicates filter type |

Returns a CRFilter object with the indicated filter type.

See also [CRFilterType](#crfiltertype)

## Other Methods

### ~~applyFilter:ToURL:~~

Filter a URL, specifying which type of filter to use.

**Declaration**

Objective-C
```C
+ (NSString *)applyFilter:(CRFilterType)filterType
                    ToURL:(NSString *)url;
```

Swift
```Swift
class func applyFilter(filterType: CRFilterType,
                        ToURL url: String)
```

**Parameters**

|   |   |
| --- | --- |
| filterType | Specifies the filter type |
| url | Specifies the url |

This capability was deprecated in 5.9.3

Returns nil when a deny filter is specified.

See also [CRFilterType](#crfiltertype)

## Constants

### CRFilterType

The filter type of CRFilter

**Declaration**

Objective-C
```C
typedef enum : NSInteger {
   CRFilterTypeScrubQuery,
   CRFilterTypeDeny,
   CRFilterTypePreserveQuery,
   CRFilterTypePreserveFragment,
   CRFilterTypePreserveParameters,
   CRFilterTypePreserveAll
} UIApplicationState;
```

Swift
```Swift
enum CRFilterType : Int {
    case ScrubQuery
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
| CRFilterTypeScrubQuery | Deprecated |
| CRFilterTypeDeny | Deny reporting for certain endpoints. Request with a URL that matches this string are not reported |
| CRFilterTypePreserveQuery | Prevent URL query string from being stripped out |
| CRFilterTypePreserveFragment | Prevent URL fragment identifier from being stripped out |
| CRFilterTypePreserveParameters | Prevent URL parameter string from being stripped out |
| CRFilterTypePreserveAll | Prevent URL query, fragment, and parameters sections from being stripped out |
