---
title: "Initialize SDK"
slug: "initialize-sdk-swift"
hidden: false
createdAt: "2019-09-11T14:17:48.136Z"
updatedAt: "2019-10-17T19:53:41.846Z"
---
Use the `start` method to initialize the Swift SDK and instantiate an instance of the Optimizely client class that exposes API methods like [Get Enabled Features](doc:get-enabled-features-swift). Each client corresponds to the datafile representing the state of a project for a certain environment.
[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.1.0
[block:api-header]
{
  "title": "Description"
}
[/block]
The constructor accepts a configuration object to configure Optimizely.

Some parameters are optional because the SDK provides a default implementation, but you may want to override these for your production environments. For example, you may want override these to set up an error handler and logger to catch issues, an event dispatcher to manage network calls, and a User Profile Service to ensure sticky bucketing.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in Swift.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**sdkKey**\n*required*",
    "0-1": "String",
    "0-2": "SDK Key",
    "2-0": "**eventDispatcher**\n*optional*",
    "2-1": "OPTEventDispatcher",
    "2-2": "A custom event handler",
    "3-0": "**userProfileService**\n*optional*",
    "3-1": "OTPUserProfileService",
    "3-2": "A custom user profile service",
    "1-0": "**logger**\n*optional*",
    "1-1": "OPTLogger",
    "1-2": "A custom logger",
    "5-1": "OptimizelyLogLevel",
    "5-0": "**defaultLogLevel**\n*optional*",
    "5-2": "Log level (default = `OptimizelyLogLevel.info`)",
    "4-0": "**periodicDownloadInterval**\n*optional*",
    "4-2": "A custom interval for periodic background datafile download in seconds (default = 10 * 60 secs)",
    "4-1": "Int"
  },
  "cols": 3,
  "rows": 6
}
[/block]

[block:api-header]
{
  "title": "Returns"
}
[/block]
Instantiates an instance of the Optimzely class.
[block:api-header]
{
  "title": "Examples"
}
[/block]
In our Swift SDK, you don't need to manage the datafile directly. The SDK includes a datafile manager that provides support for datafile polling to automatically update the datafile on a regular interval while the application is in the foreground.

To use it:
1. Create a `OptimizelyClient` by supplying your **SDK Key** and optional configuration settings. You can find the SDK key in the *Settings > Environments* tab of a Full Stack project.

2. Choose whether to start the client [synchronously or asynchronously](#section-use-synchronous-or-asynchronous-initialization) and use the appropriate `start` method to instantiate a client.
[block:code]
{
  "codes": [
    {
      "code": "// Build and config OptimizelyClient\noptimizely = OptimizelyClient(sdkKey: \"<Your_SDK_Key>\")\n        \n// Synchronously initialize the client, then activate an experiment\ndo {\n\ttry optimizely.start(datafile: datafileJSON)\n  let variationKey = try optimizely.activate(experimentKey: experimentKey,\n  \t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\tuserId: userId)        \n} catch {\n  // errors\n}  \n\n// Or, instantiate it asynchronously with a callback\noptimizely.start { result in\n\tlet variationKey = try? optimizely.activate(experimentKey: experimentKey,\n  \t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\t\tuserId: userId)\n}\n\n     ",
      "language": "swift",
      "name": "Swift"
    },
    {
      "code": "// Build and config OptimizelyClient\nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:@\"<Your_SDK_Key>\"];\n    \n// Synchronously initialize the client, then activate an experiment\nBOOL status = [self.optimizely startWithDatafile:datafileJSON error:nil];\nNSString *variationKey = [self.optimizely activateWithExperimentKey:experimentKey\n                          userId:userId \n                          attributes:attributes \n                          error:nil];\n\n// Or, instantiate it asynchronously with a callback\n[self.optimizely startWithCompletion:^(NSData *data, NSError *error) {\n\tNSString *variationKey = [self.optimizely activateWithExperimentKey:experimentKey \n                          userId:userId\n                          attributes:attributes\n                          error:nil];\n}];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
See the [Swift example: `AppDelegate.swift`](https://github.com/optimizely/swift-sdk/blob/master/DemoSwiftApp/AppDelegate.swift). We also provide an [Objective-C example: `AppDelegate.m`](https://github.com/optimizely/swift-sdk/blob/master/DemoObjCApp/AppDelegate.m).

[block:api-header]
{
  "title": "Use synchronous or asynchronous initialization"
}
[/block]
You have two choices for when to instantiate the Optimizely client: **synchronous** and **asynchronous**. The *behavioral* difference between the two methods is whether your application pings the Optimizely servers for a new datafile during initialization. The *functional* difference between the two methods is whether your app prioritizes accuracy or speed.

### Synchronous initialization

The synchronous method prioritizes speed over accuracy. Instead of attempting to download a new datafile every time you initialize an Optimizely client, your app uses a local version of the datafile. This local datafile can be cached from a previous network request.

When you initialize a client synchronously, the Optimizely manager first searches for a cached datafile. If one is available, the manager uses it to complete the client initialization. If the manager can't find a cached datafile, the manager searches for a bundled datafile. If the manager finds a the bundled datafile, it uses the datafile to complete the client initialization. If the manager can't find a bundled datafile, the manager can't initialize the client.
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
To initialize an `OptimizelyClient` object synchronously, call `optimizelyClient.start(datafile:datafile)`.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "The datafile you pass must have the same project ID that you provided when initializing the Optimizely manager."
}
[/block]
### Asynchronous initialization

The asynchronous method prioritizes accuracy over speed. During initialization, your app requests the newest datafile from the CDN servers. Requesting the newest datafile ensures that your app always uses the most current project settings, but it also means your app cannot instantiate a client until it downloads a new datafile, discovers the datafile has not changed, or until the request times out. This process takes time.

Initializing a client asynchronously executes like the [synchronous initialization](#section-synchronous-initialization), except the manager will first attempt to download the newest datafile. This network activity is what causes an asynchronous initialization to take longer to complete.

If the network request returns an error (such as when network connectivity is unreliable) or if the manager discovers that the cached datafile is identical to the newest datafile, the manager then uses the synchronous approach. If manager discovers that the datafile has been updated and now differs from the cached datafile, the manager downloads the new datafile and uses it to initialize the client.
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
To initialize an `OptimizelyClient` object asynchronously, call `optimizelyClient.start(completion:)` and pass it a callback function.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "The datafile you pass must have the same project ID that you provided when initializing the Optimizely manager."
}
[/block]

[block:api-header]
{
  "title": "Configure datafile polling"
}
[/block]
During its initialization, the manager attempts to pull the newest datafile from the CDN servers. After this, the Optimizely manager can periodically check for and pull a new datafile at the time interval you set during its initialization. The process of checking for and downloading a new datafile is called *datafile polling*.

By default, datafile polling will be set to every 10 minutes. **To disable periodic datafile polling, you must define a pass in a polling interval of 0**. This value is the number of seconds the manager waits between datafile polling attempts. 
[block:code]
{
  "codes": [
    {
      "code": "// Datafile update interval can be configured when the SDK is configured\n// Background datafile update is disabled if periodicDownloadInterval is zero\noptimizely = OptimizelyClient(sdkKey: \"<Your_SDK_Key>\",\n                              periodicDownloadInterval: 5*60)\n// every 5 minutes\n",
      "language": "swift",
      "name": "Swift"
    },
    {
      "code": "// Datafile update interval can be configured when the SDK is configured\n// Background datafile update is disabled if periodicDownloadInterval is zero\nself.optimizely = [[OptimizelyClient alloc] initWithSdkKey:@\"<Your_SDK_Key>\"\n                    logger:nil\n                    eventDispatcher:nil\n                    userProfileService:nil\n                    periodicDownloadInterval:5*60\n                    defaultLogLevel:OptimizelyLogLevelInfo];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
### Usage notes

* The minimum polling interval is 2 minutes while the app is open. If you specify shorter polling intervals than the minimum, the SDK will automatically reset the intervals to 2 minutes.
* The Optimizely manager only checks for new datafiles when the SDK is active and the app is running on iOS/tvOS. You must configure background updates in iOS using the app delegate and calling the datafile manager download.
* **If you have datafile polling enabled, the Swift SDK automatically updates when a new datafile is detected to ensure that you are always working with the latest datafile**. To be notified when the project has updated, register for a datafile change via the datafile change notification listener. The [Swift demo app](https://github.com/optimizely/swift-sdk/blob/master/DemoSwiftApp/AppDelegate.swift) shows an example where we re-run our [Is Feature Enabled](doc:is-feature-enabled-swift) method based on a new datafile.
* If you wish to have your datafile just update when the app comes to foreground, you can do the following:
  1. Set the periodicDownloadInterval = 0
  2. Just call start when coming from foreground:

[block:code]
{
  "codes": [
    {
      "code": "    func applicationWillEnterForeground(_ application: UIApplication) {\n        optimizely.start { result in\n            switch result {\n            case .failure(let error):\n                print(\"Optimizely SDK initiliazation failed: \\(error)\")\n            case .success:\n                print(\"Optimizely SDK initialized successfully!\")\n              \t// retest your features or experiments?\n            }\n\t\t\t\t\t\t\n            self.startWithRootViewController()\n        }\n    }",
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
      "code": "optimizely = OptimizelyClient(sdkKey: \"<Your_SDK_Key>\")\n\ndo {\n\tlet localDatafilePath = Bundle.main.path(forResource: “datafileName”, ofType: \"json\")!\n\tlet datafileJSON = try String(contentsOfFile: localDatafilePath, encoding: .utf8)\n\ttry optimizely.start(datafile: datafileJSON)\n\t\n\t// Optimizely SDK initialized successfully\n} catch {\n\t// Optimizely SDK initiliazation failed\n}\n\n",
      "language": "swift",
      "name": "Swift"
    },
    {
      "code": "self.optimizely = [[OptimizelyClient alloc] initWithSdkKey:@\"<Your_SDK_Key>\"];\n\nNSString *filePath = [[NSBundle mainBundle] pathForResource:@“fileName” ofType:@\"json\"]\nNSString *datafileJSON = [NSString stringWithContentsOfFile:filePath encoding:NSUTF8StringEncoding error:nil];\n\nBOOL status = [self.optimizely startWithDatafile:datafileJSON error:nil];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Swift are at [OptimizelyClient.swift](https://github.com/optimizely/swift-sdk/blob/master/Sources/Optimizely/OptimizelyClient.swift).