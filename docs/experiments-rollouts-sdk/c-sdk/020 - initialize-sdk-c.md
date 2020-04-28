---
title: "Initialize SDK"
slug: "initialize-sdk-c"
hidden: false
createdAt: "2019-08-21T21:07:18.533Z"
updatedAt: "2019-08-22T21:10:12.464Z"
---
Use the `instantiate` method to initialize the C# SDK and instantiate an instance of the Optimizely client class that exposes API methods like [Get Enabled Features](doc:get-enabled-features-c). Each client corresponds to the datafile representing the state of a project for a certain environment.
[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.2.0
[block:api-header]
{
  "title": "Description"
}
[/block]
The constructor accepts a configuration object to configure Optimizely.

Some parameters are optional because the SDK provides a default implementation, but you may want to override these for your production environments. For example, you may want override these to set up an [error handler](doc:customize-error-handler-c) and [logger](doc:customize-logger-c) to catch issues, an event dispatcher to manage network calls, and a User Profile Service to ensure sticky bucketing.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in C#.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**datafile**\n*optional* ",
    "0-1": "string",
    "0-2": "The JSON string representing the project.",
    "1-0": "**eventDispatcher**\n*optional*",
    "1-1": "IEventDispatcher",
    "1-2": "An event handler to manage network calls.",
    "2-0": "**logger**\n*optional* ",
    "2-1": "ILogger",
    "2-2": "A logger implementation to log issues.",
    "3-0": "**errorHandler**\n*optional*",
    "3-1": "IErrorHandler",
    "3-2": "An error handler object to handle errors.",
    "4-0": "**userProfileService**\n*optional*",
    "4-1": "UserProfileService",
    "4-2": "A user profile service.",
    "5-0": "**skipJsonValidation**\n*optional*",
    "5-1": "boolean",
    "5-2": "Specifies whether the JSON should be validated. Set to `true` to skip JSON validation on the schema, or `false` to perform validation."
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
In the C# SDK, you can provide either `sdkKey` or `datafile` or both.

* When initializing with just the SDK key, the SDK will poll for datafile changes in the background at regular intervals.
* When initializing with just the datafile, the SDK will NOT poll for datafile changes in the background.
* When initializing with both the SDK key and datafile, the SDK will use the given datafile and start polling for datafile changes in the background.

### Instantiate using SDK Key (recommended)

In the C# SDK, you only need to pass the SDK key value to instantiate a client. Whenever the experiment configuration changes, the SDK handles the change for you.

Include `sdkKey` as a string property in the options object you pass to the `createInstance` method.

When you provide the `sdkKey`, the SDK instance downloads the datafile associated with that `sdkKey`. When the download completes, the SDK instance updates itself to use the downloaded datafile.
[block:code]
{
  "codes": [
    {
      "code": "ProjectConfigManager projectConfigManager =\n        new HttpProjectConfigManager.Builder()\n\t .WithSdkKey(sdkKey)\n\t .WithPollingInterval(TimeSpan.FromMinutes(1))\n\t .Build();\n\n",
      "language": "csharp"
    }
  ]
}
[/block]
### Instantiate using datafile

You can also instantiate with a hard-coded datafile. If you don't pass in an SDK key, the Optimizely Client will not automatically sync newer versions of the datafile. Any time you retrieve an updated datafile, just re-instantiate the same client.

For simple applications, all you need to provide to instantiate a client is a [datafile](doc:get-the-datafile) specifying the project configuration for a given environment. For most advanced implementations, you'll want to [customize the logger](doc:customize-logger-c) or [error handler](doc:customize-error-handler-c) for your specific requirements.
[block:code]
{
  "codes": [
    {
      "code": "using OptimizelySDK;\n\n// Instantiate an Optimizely client\nOptimizely OptimizelyClient = new Optimizely(datafile);\n\n",
      "language": "csharp"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for C# are at [Optimizely.cs](https://github.com/optimizely/csharp-sdk/blob/master/OptimizelySDK/Optimizely.cs).