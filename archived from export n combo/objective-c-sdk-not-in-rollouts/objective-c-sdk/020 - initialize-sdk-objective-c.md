---
title: "Initialize SDK"
slug: "initialize-sdk-objective-c"
hidden: true
createdAt: "2019-09-11T14:16:48.023Z"
updatedAt: "2019-09-13T12:00:50.217Z"
---
In our Objective-C SDK, you don't need to manage the datafile directly. The SDK has an additional abstraction called a **manager** that handles getting the datafile and instantiating the client for you during the SDK's initialization process. The manager includes:
* A built-in [User Profile Service](doc:implement-a-user-profile-service-objective-c) for storing variation assignments on the device.
* A built-in [event dispatcher](doc:customize-event-dispatcher-objective-c) for sending events in the background. It also retries sending using exponential backup if it fails.
* Support for datafile polling to automatically update the datafile on a regular interval while the application is in the foreground.

The manager is implemented as a factory for Optimizely client instances. To use it:
1. Create a manager object by supplying your **SDK Key** and optional configuration settings for datafile polling and datafile bundling. You can find the SDK key in the *Settings > Environments* tab of a Full Stack project.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Earlier versions of the SDK used a project ID rather than an SDK Key to create a manager. Project IDs are still supported in 2.x for backwards compatibility. If you supply a project ID, the SDK retrieves the **primary** environment's datafile. We recommend using SDK keys because they enable you to retrieve datafiles for other environments."
}
[/block]
2. Choose whether to instantiate the client synchronously or asynchronously and use the appropriate `initialize` method to instantiate a client.
[block:code]
{
  "codes": [
    {
      "code": "// In AppDelegate.m\n#import <OptimizelySDKiOS/OptimizelySDKiOS.h>\n\n// Build a manager\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n        builder.sdkKey = @\"SDK_KEY_HERE\";\n    }]];\n\n\n// Instantiate a client synchronously using cached datafile\nOPTLYClient *client = [manager initialize];\nOPTLYVariation *variation = [client activate:@\"experimentKey\"\n                                    userId:@\"userId\"\n                                    attributes:nil];\n\n// Or, instantiate it asynchronously with a callback\n[manager initializeWithCallback:^(NSError * _Nullable error,\n                                  OPTLYClient * _Nullable client) {\n  OPTLYVariation *variation = [client activate:@\"experimentKey\"\n                                      userId:@\"userId\"\n                                      attributes:nil];\n}];\n\n",
      "language": "objectivec"
    },
    {
      "code": "// In AppDelegate.swift\nimport OptimizelySDKiOS\n\n// Build a manager\nlet optimizelyManager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n            builder?.sdkKey = \"SDK_KEY_HERE\"\n        }))\n// Synchronously initialize the client, then activate an experiment\nlet optimizelyClient : OPTLYClient? = optimizelyManager?.initialize(withDatafile:jsonDatafile!)\nlet variation = optimizelyClient?.activate(\"experimentKey\",\n                                           userId:\"userId\",\n                                           attributes:nil)\n\n// Or, instantiate it asynchronously with a callback\noptimizelyManager?.initialize(callback: { [weak self] (error, optimizelyClient) in\n  let variation = optimizelyClient?.activate(\"experimentKey\",\n                                             userId:\"userId\",\n                                             attributes:nil)\n})\n\n",
      "language": "swift",
      "name": "Swift"
    }
  ]
}
[/block]
For full examples, see:
- [Objective-C example: `AppDelegate.m`](https://gist.github.com/mauerbac/8243dce3884b460271a0ca9673f84fae)
- [Swift example: `AppDelegate.swift`](https://gist.github.com/mauerbac/4dd686a390b6cb305a358844568c0eb4)
[block:api-header]
{
  "title": "Get a Client"
}
[/block]
Each manager retains a reference to its most recently initialized client. You can use the `getOptimizely` function to return that instance.

If this method is called before any client objects have been initialized, a dummy client instance is created and returned. This dummy instance logs errors when any of its methods are used.
[block:code]
{
  "codes": [
    {
      "code": "let optimizelyClient = [manager getOptimizely];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let optimizelyClient = optimizelyManager?.getOptimizely();\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The 3.0.x mobile SDK documentation uses the term `OptimizelyManager`. To be consistent with other Full Stack SDKs, this has been changed to the `OptimizelyClient`  in the Objective-C SDK."
}
[/block]

[block:api-header]
{
  "title": "Use synchronous or asynchronous initialization"
}
[/block]
You have two choices for when to instantiate the Optimizely client: **synchronous** and **asynchronous**. The *behavioral* difference between the two methods is whether your application pings the Optimizely servers for a new datafile during initialization. The *functional* difference between the two methods is whether your app prioritizes speed (synchronous method) or accuracy (asynchronous method).

### Synchronous initialization

The synchronous start does not access the CDN at startup time. By default, the SDK will attempt to update the cache in the background. Instead of attempting to download a new datafile every time you initialize an Optimizely client, your app uses a local version of the datafile. This local datafile can be cached from a previous network request.

When you initialize a client synchronously, the SDK first searches for a cached datafile. If one is available, the SDK uses it to complete the client initialization. If the SDK can't find a cached datafile, the SDK searches for a bundled datafile. If the SDK finds a the bundled datafile, it uses the datafile to complete the client initialization. If the SDK can't find a bundled datafile, the SDK can't initialize the client.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/1085e10-initialize-synchronous.png",
        "initialize-synchronous.png",
        1200,
        760,
        "#ccd8e1"
      ],
      "sizing": "80",
      "border": true
    }
  ]
}
[/block]
### Asynchronous initialization

The asynchronous method always attempts to get a new version from the CDN. You can pass in a timeout interval; see [the documentation for `urlsessionconfiguration.timeoutIntervalForResource`](https://developer.apple.com/documentation/foundation/urlsessionconfiguration/1408153-timeoutintervalforresource). Requesting the newest datafile ensures that your app always uses the most current project settings, but it also means your app cannot instantiate a client until it downloads a new datafile, discovers the datafile has not changed, or until the request times out. This has the extra step of checking the CDN.

Initializing a client asynchronously executes like the synchronous initialization, except the SDK will first attempt to download the newest datafile. 

If the network request returns an error (such as when network connectivity is unreliable) or if the SDK discovers that the cached datafile is identical to the newest datafile, the SDK then uses the synchronous approach. If the SDK discovers that the datafile has been updated and now differs from the cached datafile, the SDK downloads the new datafile and uses it to initialize the client.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ee8b24d-initialize-asynchronous.png",
        "initialize-asynchronous.png",
        1013,
        1200,
        "#c8d7de"
      ],
      "sizing": "80",
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Configure datafile polling"
}
[/block]
To initialize an `OPTLYClient` object **synchronously**, call `[manager initialize]` or `[manager initializeWithDatafile:datafile]`.

To initialize an `OPTLYClient` object **asynchronously**, call `[manager initializeWithCallback:callback]` and pass it a callback function.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "The datafile you pass must have the same project ID that you provided when initializing the Optimizely manager."
}
[/block]
During its initialization, the manager attempts to pull the newest datafile from the CDN servers. After this, the Optimizely manager can periodically check for and pull a new datafile at the time interval you set during its initialization. The process of checking for and downloading a new datafile is called *datafile polling*.

By default, the manager only checks for a new datafile during initialization. **To enable periodic datafile polling, you must define a polling interval when building the manager**. This value is the number of seconds the manager waits between datafile polling attempts. 
[block:code]
{
  "codes": [
    {
      "code": "OPTLYDatafileManagerDefault *datafileManager = [[OPTLYDatafileManagerDefault alloc] initWithBuilder:[OPTLYDatafileManagerBuilder builderWithBlock:^(OPTLYDatafileManagerBuilder * _Nullable builder) {\n  builder.datafileConfig = [[OPTLYDatafileConfig alloc] initWithProjectId:nil withSDKKey:@\"SDK_KEY_HERE\"];\n  builder.datafileFetchInterval = 120.0;\n}]];\n// Build a manager\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n  builder.sdkKey = @\"SDK_KEY_HERE\";\n  builder.datafileManager = datafileManager;\n}]];\n\n",
      "language": "objectivec"
    },
    {
      "code": "// ---- Create the Datafile Manager ----\nlet datafileManager = OPTLYDatafileManagerDefault(builder:      OPTLYDatafileManagerBuilder(block: { (builder) in\n  builder!.datafileFetchInterval =   TimeInterval(self.datafileManagerDownloadInterval)\n  builder!.datafileConfig = OPTLYDatafileConfig(projectId: nil, withSDKKey:\"SDK_KEY_HERE\")!;\n}))\n\nlet builder = OPTLYManagerBuilder(block: { (builder) in\n  builder!.projectId = nil;\n  builder!.sdkKey = \"SDK_KEY_HERE\"\n  builder!.datafileManager = datafileManager!\n  builder!.eventDispatcher = eventDispatcher\n})\n\n// ---- Create the Manager ----\nvar optimizelyManager = OPTLYManager(builder: builder)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Usage notes

* In the Objective-C SDK, the minimum polling interval is 2 minutes while the app is open. If you specify a shorter polling interval, the SDK will automatically reset the intervals to 2 minutes.

* Updated datafiles do not take effect until your app is restarted or when you re-initialize the Optimizely manager. This implementation strategy allows the data to change while the app is running without causing nondeterministic behavior.

The Optimizely manager only checks for new datafiles when the SDK is active and the app is running on iOS/tvOS. You must configure background updates in iOS using the app delegate and calling the datafile manager download.

When your customer opens your app for the first time after installing or updating it, the manager attempts to pull the newest datafile from Optimizely's CDN. If your app is unable to contact the servers, the manager can substitute a datafile that you included (_bundled_) when you created your app.

By bundling a datafile within your app, you ensure that the Optimizely manager can initialize without waiting for a response from the CDN. This lets you prevent poor network connectivity from causing your app to hang or crash while it loads.

Datafile bundling works with both synchronous and asynchronous initialization because reverting to a bundled datafile happens during the Optimizely manager's initialization, before a client is instantiated.
[block:code]
{
  "codes": [
    {
      "code": "OPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n  // Load the datafile from the bundle\n  NSString *filePath =[[NSBundle bundleForClass:[self class]]\n                       pathForResource:@\"datafileName\"\n                       ofType:@\"json\"];\n  NSString *fileContents =[NSString stringWithContentsOfFile:filePath\n                           encoding:NSUTF8StringEncoding\n                           error:nil];\n  NSData *jsonData = [fileContents dataUsingEncoding:NSUTF8StringEncoding];\n\n  // Set the datafile in the builder\n  builder.datafile = jsonData;\n  builder.sdkKey = @\"SDK_KEY_HERE\";\n}]];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder?.sdkKey = \"SDK_KEY_HERE\"\n  // Load the datafile from the bundle\n  let bundle = Bundle.init(for: self.classForCoder)\n  let filePath = bundle.path(forResource: \"datafileName\", ofType: \"json\")\n  var jsonDatafile: Data? = nil\n  do {\n    jsonDatafile = try Data(contentsOf: URL(fileURLWithPath: filePath!), options: .mappedIfSafe)\n  }\n  catch {\n  \tprint(\"Invalid JSON data\")\n  }\n  // Set the datafile in the builder\n  builder!.datafile = jsonDatafile\n}))\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Enable bundled datafiles"
}
[/block]
When your customer opens your app for the first time after installing or updating it, the manager attempts to pull the newest datafile from Optimizely's CDN. If your app is unable to contact the servers, the manager can substitute a datafile that you included (_bundled_) when you created your app.

By bundling a datafile within your app, you ensure that the Optimizely manager can initialize without waiting for a response from the CDN. This lets you prevent poor network connectivity from causing your app to hang or crash while it loads.

Datafile bundling works with both synchronous and asynchronous initialization because reverting to a bundled datafile happens during the Optimizely manager's initialization, before a client is instantiated.
[block:code]
{
  "codes": [
    {
      "code": "OPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n  // Load the datafile from the bundle\n  NSString *filePath =[[NSBundle bundleForClass:[self class]]\n                       pathForResource:@\"datafileName\"\n                       ofType:@\"json\"];\n  NSString *fileContents =[NSString stringWithContentsOfFile:filePath\n                           encoding:NSUTF8StringEncoding\n                           error:nil];\n  NSData *jsonData = [fileContents dataUsingEncoding:NSUTF8StringEncoding];\n\n  // Set the datafile in the builder\n  builder.datafile = jsonData;\n  builder.sdkKey = @\"SDK_KEY_HERE\";\n}]];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder?.sdkKey = \"SDK_KEY_HERE\"\n  // Load the datafile from the bundle\n  let bundle = Bundle.init(for: self.classForCoder)\n  let filePath = bundle.path(forResource: \"datafileName\", ofType: \"json\")\n  var jsonDatafile: Data? = nil\n  do {\n    jsonDatafile = try Data(contentsOf: URL(fileURLWithPath: filePath!), options: .mappedIfSafe)\n  }\n  catch {\n  \tprint(\"Invalid JSON data\")\n  }\n  // Set the datafile in the builder\n  builder!.datafile = jsonDatafile\n}))\n\n",
      "language": "swift"
    }
  ]
}
[/block]