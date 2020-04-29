---
title: "Migrate to Swift SDK"
slug: "migrate-to-swift-sdk"
hidden: true
createdAt: "2019-09-11T14:02:06.505Z"
updatedAt: "2019-09-13T11:56:44.852Z"
---
This guide describes how to migrate existing applications that use the Optimizely Full Stack Objective-C SDK to the newly released [Swift SDK](https://github.com/optimizely/swift-sdk).
[block:api-header]
{
  "title": "Initialization"
}
[/block]
Compared to the Objective-C SDK, the Swift SDK simplifies the steps for SDK initialization for both [synchronous](#section-synchronous-initialization) and [asynchronous](#section-asynchronous-initialization) modes.

### Asynchronous initialization

For the Swift SDK, start with `import optimizely` for all platforms (instead of importing platform-specific framework names as in the Objective-C SDK, `import OptimizelySDKiOS` or `import OptimizelySDKtvOS`).

The hierarchy of `OPLTYManager` and `OPTLYClient` in the Objective-C SDK is a single layer of `OptimizelyClient` in the Swift SDK. The builder pattern for the Objective-C initialization has been removed as well.

As a result, the SDK module of the Swift SDK can be instantiated with a single instruction, `Optimizely(sdkKey:)`, with a parameter of `sdkKey`.

After `OptimizelyClient` is instantiated, it should be explicitly started with a completion handler by calling `OptimizelyClient.start(completionHandler:)`. If starting the SDK fails due to any error, `OptimizelyClient` will be set to the error state and terminate all subsequent API calls gracefully.

The following code examples show asynchronous initialization in the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "import optimizely\n\n// Build and config OptimizelyClient\noptimizely = OptimizelyClient(sdkKey: \"SDK_KEY_HERE\")\n        \n// Instantiate it asynchronously with a callback\noptimizely.start { result in\n    // initialization completed\n}\n\n",
      "language": "swift"
    },
    {
      "code": "@import optimizely;\n\n// Build and config OptimizelyClient\nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:@\"SDK_KEY_HERE\"];\n    \n// Or, instantiate it asynchronously with a callback\n[self.optimizely startWithCompletion:^(NSData *data, NSError *error) {\n    // initialization completed\n}];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

The following code examples show asynchronous initialization in the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "import OptimizelySDKiOS\n\n// Build a manager\nlet manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n       builder?.sdkKey = \"SDK_KEY_HERE\"\n    }))\n\n// Instantiate it asynchronously with a callback\nmanager?.initialize(callback: { (error, optimizelyClient) in\n   // initialization completed\n})\n\n",
      "language": "swift"
    },
    {
      "code": "#import <OptimizelySDKiOS/OptimizelySDKiOS.h>\n\n// Build a manager\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n        builder.sdkKey = @\"SDK_KEY_HERE\";\n    }]];\n\n// Instantiate it asynchronously with a callback\n[manager initializeWithCallback:^(NSError * _Nullable error,\n                                  OPTLYClient * _Nullable client) {\n    // initialization completed\n}];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

### Synchronous initialization

After the Swift SDK is instantiated, it can be started synchronously with a given project datafile by calling `OptimizelyClient.start(datafile:)`. This step is similar to instantiation in the Objective-C SDK. 

One important change is that the Swift SDK throws an error (`OptimizelyError`) when the SDK initialization fails for any reason. This allows applications to take proper actions according to error type. If starting the SDK fails due to any error, `OptimizelyClient` will be set to the error state and terminate all subsequent API calls gracefully.

The following code examples show synchronous initialization in the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "// Build and config OptimizelyClient\noptimizely = OptimizelyClient(sdkKey: \"SDK_KEY_HERE\")\n        \n// Instantiate a client synchronously using a given datafile\ndo {\n   try optimizely.start(datafile: datafileJSON)    \n} catch {\n   // errors\n}\n\n",
      "language": "swift"
    },
    {
      "code": "// Build and config OptimizelyClient\nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:@\"SDK_KEY_HERE\"];\n    \n// Instantiate a client synchronously using a given datafile\nBOOL status = [self.optimizely startWithDatafile:datafileJSON error:nil];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
The following code examples show synchronous initialization in the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "import OptimizelySDKiOS\n\n// Build a manager\nlet manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n            builder?.sdkKey = \"SDK_KEY_HERE\"\n            Builder?.datafile = datafileJSON\n        }))\n\n// Instantiate a client synchronously using a given datafile\nlet optimizelyClient : OPTLYClient? = manager?.initialize()\n\n",
      "language": "swift"
    },
    {
      "code": "#import <OptimizelySDKiOS/OptimizelySDKiOS.h>\n\n// Build a manager\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n        builder.sdkKey = @\"SDK_KEY_HERE\";\n        Builder.datafile = datafileJSON;\n    }]];\n\n// Instantiate a client synchronously using a given datafile\nOPTLYClient *client = [manager initialize];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Feature and experiment APIs"
}
[/block]
The Swift SDK provides the same set of APIs for features and experiments as the Objective-C SDK and provides the same functionality. 

Some of the APIs are upgraded with Swift-native error handling support. When the SDK detects errors (such as invalid experiment keys or SDK not ready), the Swift SDK throws properly typed errors (OptimizelyError). This is different from the Objective-C SDK, which returns nil on error. In this way, the Swift SDK returns more information about detected errors so that applications can take proper action according to the error type. If specific error handling is not necessary, those APIs can be called with “try?” to suppress error throwing and then the Swift SDK will return nil for errors like the Objective-C SDK.

### Activate

The Swift SDK throws an OptimizelyError when any error is detected while processing the `activate` call. When the API completes successfully, it returns the key value (string) of the selected variation (the Objective-C SDK returns an entire object of the selected variation).

Other than these changes, the Swift SDK functionality is same as that of the Objective-C SDK (except for a slight change in parameter labels).

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\n// Activate an A/B test\ndo {\n  let variationKey = try optimizely.activate(experimentKey: \"experiment\", userId: \"user_123\", attributes: attributes) {\n  if variationKey == \"control\" {\n     // Execute code for \"control\" variation\n  } else {\n     // Execute code for other variation\n  }\n  \n} catch {\n   // Execute code for users who don't qualify for the experiment \n   // or for errors detected\n}\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\n// Activate an A/B test\nNSString *variationKey = [optimizely activateWithExperimentKey:@\"experiment\"\n                                  userId:@\"user_123\"\n                              attributes:attributes\n                                  error:nil];\n...\n\n  ",
      "language": "objectivec"
    }
  ]
}
[/block]
Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\n// Activate an A/B test\nif let variation = optimizelyClient.activate(\"experiment\",\n                           userId:\"user_123\",      \n                           attributes:attributes){\n   if variation.key == “control {\n       // Execute code for \"control\" variation\n   } else {\n       // Execute code for other variation\n   }\n} else {\n   // Execute code for users who don't qualify for the experiment or\n   // for errors detected\n}\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\n// Activate an A/B test\nOPTLYVariation *variation = [optimizelyClient activate:@\"experiment\", \nuserId:@\"user_123\",\n                                            attributes:attributes];\n\n...\n\n  ",
      "language": "objectivec"
    }
  ]
}
[/block]
### isFeatureEnabled

In the Swift SDK, the `isFeatureEnabled` API is identical to that of the Objective-C SDK (except for a slight change in parameter labels). Note that `isFeatureEnabled` returns `false` instead of throwing an error when errors are detected.

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\n// Evaluate a feature flag\nlet enabled = optimizely.isFeatureEnabled(featureKey: \"feature\", userId:\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\n// Evaluate a feature flag\nBOOL enabled = [optimizely isFeatureEnabledWithFeatureKey:@\"feature\"\nuserId:@\"user_123\",\n                                       attributes:attributes];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\n// Evaluate a feature flag\nlet enabled = optimizelyClient.isFeatureEnabled(\"feature\", userId:\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\n// Evaluate a feature flag\nbool enabled = [optimizelyClient isFeatureEnabled:@\"feature\",\n                                           userId:@\"user_123\",\n                                       attributes:attributes];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
### getFeatureVariable

When any error is detected and the value of the feature variable cannot be determined, the Swift SDK throws an `OptimizelyError`. Other than that, the `getFeatureVariable` functionality is same as in the Objective-C SDK (except for a slight change in parameter labels).

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\ndo {\n\n  let featureVariableValue = try optimizely.getFeatureVariableString(featureKey: \"feature\", variableKey: \"variable\", userId: \"user_123\", attributes:attributes)                                                                             \n} catch {\n   // error\n}\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nNSString *featureVariableValue = [optimizely getFeatureVariableStringWithFeatureKey: @\"variable\"\t\tvariableKey:@\"variable”\t\t\t\t\tuserId:@\"user_123\"\n       attributes:attributes\t\t\t\t\t\terror:nil];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet featureVariableValue = optimizelyClient.getFeatureVariableString(\"feature\", variableKey:”variable\", userId:\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nNSString *featureVariableValue = [optimizelyClient getFeatureVariableString:@\"feature\",                                                           variableKey:@”variable\",                                                                  userId:@\"user_123\",                                                              attributes:attributes];\n                                  \n                                  ",
      "language": "objectivec"
    }
  ]
}
[/block]
### track

The Swift SDK throws an `OptimizelyError` when any error is detected while processing event tracking. Other than that, the `track` functionality is same as in the Objective-C SDK (except for a slight change in parameter labels). 

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet tags = [\n  \"category\": \"shoes\",\n  \"count\": 2,\n]\n\n// Track a conversion event for the provided user with attributes\ntry? optimizely.track(eventKey: \"event\", userId: \"user_123\", attributes: attributes, eventTags: eventTags)\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nNSDictionary *tags = @{\n  @\"category\" : @\"shoes\",\n  @\"count\": @5\n};\n\n// Track a conversion event for the provided user with attributes\n[optimizely trackWithEventKey:@\"event\"\n                               userId:@\"user_123\"\n                           attributes:attributes\n                            eventTags:tags\n                                error:nil];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet tags = [\n  \"category\": \"shoes\",\n  \"count\": 2,\n]\n\noptimizelyClient.track(\"event\", userId:\"user_123\", attributes:attributes, eventTags:tags);\n\n",
      "language": "swift"
    },
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nNSDictionary *tags = @{\n  @\"category\" : @\"shoes\",\n  @\"count\": @5\n};\n\n[optimizelyClient track:@\"event\"\n                 userId:@\"user_123\"\n             attributes:attributes\n              eventTags:tags];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Customization"
}
[/block]
Just like the Objective-C SDK, the Swift SDK allows you to customize key service modules including `Logger`, `EventDispatcher`, and `UserProfileService`. The Swift SDK does not include support for custom `ErrorHandlers` because errors are forwarded to applications via error throw.

Customization is optional. The Swift SDK implements full-featured default versions of these modules, serving the demands of most applications.

Custom modules are passed as optional parameters when the Swift SDK is initialized. When custom versions are not provided, the default modules are used automatically.

### Logger

The Swift SDK logs all messages with `os_log()` by default, which writes messages to the device console with proper level controls and categories. 

If necessary, logger functionality can be replaced with a custom version that complies with the `OPTLogger` protocol.

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "// create OptimizelyClient with a custom Logger\nlet customLogger = CustomLogger()\n\noptimizely = OptimizelyClient(sdkKey: \"SDK_KEY_HERE\",\n                              logger: customLogger)\n                              \n                              ",
      "language": "swift"
    },
    {
      "code": "// create OptimizelyClient with a custom Logger\nCustomLogger *customLogger = [[CustomLogger alloc] init];\n     \nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:@\"SDK_KEY_HERE\"\n                            \t\t\t\tlogger:customLogger\n                            \t\t\t\teventDispatcher:nil\n                            \t\t\t\tuserProfileService:nil\n                            \t\t\t\tperiodicDownloadInterval:nil\n                            \t\t\t\tdefaultLogLevel:OptimizelyLogLevelInfo];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let customLogger = CustomLogger()\n\nlet manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder?.sdkKey = \"SDK_KEY_HERE\"\n  builder?.logger = customLogger\n}))\n\n",
      "language": "swift"
    },
    {
      "code": "CustomLogger *customLogger = [[CustomLogger alloc] init];\n\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n  builder.sdkKey = @\"SDK_KEY_HERE\";\n  builder.logger = customLogger;\n}]];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
### EventDispatcher

The Swift SDK implements a default version of a full-featured event dispatcher. All events are saved into permanent storage and removed only when they have been successfully delivered to the server. This process guarantees that events won’t be lost even when network connections fail or applications crash.

If necessary, the event dispatcher functionality can be replaced with a custom version that complies with the `OPTEventDispatcher` protocol.

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "// create OptimizelyClient with a custom EventDispatcher\nlet customEventDispatcher = CustomEventDispatcher()\n      \noptimizely = OptimizelyClient(sdkKey: sdkKey,\n                              eventDispatcher: customEventDispatcher)\n                              \n                              ",
      "language": "swift"
    },
    {
      "code": "// create OptimizelyClient with a custom EventDispatcher\nCustomEventDispatcher *customEventDispatcher = [[CustomEventDispatcher alloc] init];\n     \nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:kOptimizelySdkKey\n                       \t\t\tlogger:nil\n                       \t\t\teventDispatcher:customEventDispatcher\n                       \t\t\tuserProfileService:nil\n                         \t\tperiodicDownloadInterval:nil                       \n                       \t\t\tdefaultLogLevel:OptimizelyLogLevelInfo];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let customEventDispatcher = CustomEventDispatcher()\n\nlet manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder?.sdkKey = \"SDK_KEY_HERE\"\n  builder?.eventDispatcher = customEventDispatcher\n }))\n\n",
      "language": "swift"
    },
    {
      "code": "CustomEventDispatcher *customEventDispatcher = [[CustomEventDispatcher alloc] init];\n\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n      builder.sdkKey = @\"SDK_KEY_HERE\";\n      builder.eventDispatcher = customEventDispatcher;\n    }]];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
### UserProfileService

The Swift SDK implements a default version of an UserProfileService, which saves experiment decisions for users in permanent storage and provides consistent results.

If necessary, the functionality can be replaced with a custom version complying to the `OPTUserProfileService` protocol.

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "// create OptimizelyClient with a custom UserProfileService\nlet customUserProfileService = CustomUserProfileService()\n\nlet optimizely = OptimizelyClient(sdkKey: \"SDK_KEY_HERE\",\n                              userProfileService: customUserProfileService)\n                              \n                              ",
      "language": "swift"
    },
    {
      "code": "// create OptimizelyClient with a custom UserProfileService\nCustomUserProfileService *customUserProfileService = [[CustomUserProfileService alloc] init];\n     \nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:@\"SDK_KEY_HERE\"\n                            \t\t\t\tlogger:nil\n                            \t\t\t\teventDispatcher:nil\n                            \t\t\t\tuserProfileService:customUserProfileService\n                            \t\t\t\tperiodicDownloadInterval:nil\n                            \t\t\t\tdefaultLogLevel:OptimizelyLogLevelInfo];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let customUserProfileService = CustomUserProfileService()\n\nlet manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder?.sdkKey = \"SDK_KEY_HERE\"\n  builder?.eventDispatcher = customUserProfileService\n }))\n\n",
      "language": "swift"
    },
    {
      "code": "CustomUserProfileService *customUserProfileService = [[CustomUserProfileService alloc] init];\n\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n      builder.sdkKey = @\"SDK_KEY_HERE\";\n      builder.userProfileService = customUserProfileService;\n    }]];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Notification Listeners"
}
[/block]
The Swift SDK provides the same notification listener APIs as the Objective-C SDK (v3.1.0) except for slight changes in parameter labels, allowing straightforward migration of notification listeners.

### Activate Notifications

The Activate Notifications API is deprecated. Use **DecisionService Notifications** instead.
`
### DecisionService Notifications

The new APIs for **DecisionService Notifications** were recently added to the Objective-C SDK (v3.1.0). If applications already use the new APIs for the Objective-C SDK, migration to the Swift SDK is straightforward because it provides identical APIs (except for a slight change in parameter labels).

If applications use the earlier APIs for Activate Notifications, replace them with the **DecisionService Notifications** API.

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "// Add a notification listener\nlet notificationId = optimizely.notificationCenter.addDecisionNotificationListener(decisionListener: { (type, userId, attributes, decisionInfo) in\n     // process data here\n})  \n\n// Remove a specific notification listener\noptimizely.notificationCenter.removeNotificationListener(notificationId: notificationId!)\n\n// Remove notification listeners of a certain type\noptimizely.notificationCenter.clearNotificationListeners(type: .decision)\n\n// Remove all notification listeners\noptimizely.notificationCenter.clearAllNotificationListeners()   \n\n",
      "language": "swift"
    },
    {
      "code": "// Add a notification listener\nNSNumber *notificationId = [self.optimizely.notificationCenter addDecisionNotificationListenerWithDecisionListener:^(NSString *type, NSString *userId, NSDictionary<NSString *,id> *attributes, NSDictionary<NSString *,id> *decisionInfo) {\n     // process data here\n}];\n\n// Remove a specific listener\n[optimizely.notificationCenter removeNotificationListenerWithNotificationId:notificationId];\n\n// Remove all of one type of listener\n[optimizely.notificationCenter clearNotificationListenersWithType:NotificationTypeDecision];\n\n// Remove all notification listeners\n[optimizely.notificationCenter clearAllNotificationListeners];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "// Add a notification listener\nlet notificationId = optimizely.notificationCenter?.addDecisionNotificationListener({ (type, userId, attributes, decisionInfo) in\n    // process data here\n})\n\n// Remove a specific notification listener\noptimizely.notificationCenter?.removeNotificationListener(UInt(notificationId!))\n\n// Remove notification listeners of a certain type\noptimizely.notificationCenter?.clearNotificationListeners(OPTLYNotificationType.decision)\n\n// Remove all notification listeners\noptimizely.notificationCenter?.clearAllNotificationListeners()\n\n",
      "language": "swift"
    },
    {
      "code": "// Add a notification listener\nNSInteger notificationId = [optimizely.notificationCenter addDecisionNotificationListener:^(NSString * _Nonnull type, NSString * _Nonnull userId, NSDictionary<NSString *,id> * _Nullable attributes, NSDictionary<NSString *,id> * _Nonnull decisionInfo) {\n    // process data here\n}];\n\n// Remove a specific listener\n[optimizely.notificationCenter removeNotificationListener:notificationId];\n\n// Remove all of one type of listener\n[optimizely.notificationCenter clearNotificationListeners:OPTLYNotificationTypeDecision];\n\n// Remove all notification listeners\n[optimizely.notificationCenter clearAllNotificationListeners];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
### Track Notifications

In the Swift SDK, the APIs for adding and removing a track notification listener are identical to those of the Objective-C SDK (except for a slight change in parameter labels).

Code examples for the **Swift SDK**:
[block:code]
{
  "codes": [
    {
      "code": "let notificationId = optimizely.notificationCenter.addTrackNotificationListener(trackListener: { (eventKey, userId, attributes, eventTags, event) in\n         // process data here\n})\n\n// see “DecisionService Notifications” for remove examples\n\n",
      "language": "swift"
    },
    {
      "code": "NSNumber *notifId = [self.optimizely.notificationCenter addTrackNotificationListenerWithTrackListener:^(NSString *eventKey,\n                                                                                                  NSString *userId,\n                                                                                                  NSDictionary<NSString *,id> *attributes, NSDictionary<NSString *,id> *eventTags, NSDictionary<NSString *,id> *event) {\n         // process data here  \n}];\n\n// see “DecisionService Notifications” for remove examples\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

Code examples for the **legacy Objective-C SDK**:
[block:code]
{
  "codes": [
    {
      "code": "// Add a track notification listener\nlet notificationId = optimizely.notificationCenter?.addTrackNotificationListener({ (eventKey, userId, attributes, eventTags, event) in\n         // process data here\n})\n\n// see “DecisionService Notifications” for remove examples\n\n",
      "language": "swift"
    },
    {
      "code": "// Add a Track notification listener\nNSInteger notificationId = [optimizely.notificationCenter addTrackNotificationListener:^(NSString * _Nonnull eventKey, NSString * _Nonnull userId, NSDictionary<NSString *,NSString *> * _Nonnull attributes, NSDictionary * _Nonnull eventTags, NSDictionary<NSString *,NSObject *> * _Nonnull event) {\n         // process data here\n}];\n\n// see “DecisionService Notifications” for remove examples\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]