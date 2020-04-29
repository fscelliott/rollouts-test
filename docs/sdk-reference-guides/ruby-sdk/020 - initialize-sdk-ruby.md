---
title: "Initialize SDK"
slug: "initialize-sdk-ruby"
hidden: false
createdAt: "2019-08-21T21:52:31.973Z"
updatedAt: "2019-08-21T21:52:40.323Z"
---
Use the `instantiate` method to initialize the Ruby SDK and instantiate an instance of the Optimizely client class that exposes API methods like [Get Enabled Features](doc:get-enabled-features-ruby). Each client corresponds to the datafile representing the state of a project for a certain environment.
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

Some parameters are optional because the SDK provides a default implementation, but you may want to override these for your production environments. For example, you may want override these to set up an [error handler](doc:customize-error-handler-ruby) and [logger](doc:customize-logger-ruby) to catch issues, an event dispatcher to manage network calls, and a User Profile Service to ensure sticky bucketing.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in Ruby.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**datafile**\n*optional* ",
    "0-1": "string",
    "0-2": "The JSON string representing the project.",
    "1-0": "**event_dispatcher**\n*optional*",
    "1-1": "EventDispatcher",
    "1-2": "An event handler to manage network calls.",
    "2-0": "**logger**\n*optional* ",
    "2-1": "Logger",
    "2-2": "A logger implementation to log issues.",
    "3-0": "**error_handler**\n*optional*",
    "3-1": "ErrorHandler",
    "3-2": "An error handler object to handle errors.",
    "4-0": "**user_profile_service**\n*optional*",
    "4-1": "UserProfileService",
    "4-2": "A user profile service.",
    "5-0": "**skip_json_validation**\n*optional*",
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
  "title": "Example"
}
[/block]
In the Ruby SDK, you can provide either `sdkKey` or `datafile` or both.

* When initializing with just the SDK key, the SDK will poll for datafile changes in the background at regular intervals.
* When initializing with just the datafile, the SDK will NOT poll for datafile changes in the background.
* When initializing with both the SDK key and datafile, the SDK will use the given datafile and start polling for datafile changes in the background.

### Instantiate using SDK Key (recommended)

In the Ruby SDK, you only need to pass the SDK key value to instantiate a client. Whenever the experiment configuration changes, the SDK handles the change for you.

Include `sdkKey` as a string property in the options object you pass to the `createInstance` method.

When you provide the `sdkKey`, the SDK instance downloads the datafile associated with that `sdkKey`. When the download completes, the SDK instance updates itself to use the downloaded datafile.
[block:code]
{
  "codes": [
    {
      "code": "# Starting in 3.2+ there are convienence factory methods for instantiating clients\nrequire 'optimizely/optimizely_factory'\n\n# Instantiate with just the SDK key. The SDK will pull the datafile remotely.\nsdk_key = 'AWDj34sdlfklsdfks'\noptimizely_client = Optimizely::OptimizelyFactory.default_instance(sdk_key)\n\n# You can optionally instantiate with a hard-coded datafile as well \ndatafile = '{ revision: \"42\" }'\noptimizely_client = Optimizely::OptimizelyFactory.default_instance(sdk_key, datafile)\n\n",
      "language": "ruby"
    }
  ]
}
[/block]
### Instantiate using datafile

You can also instantiate with a hard-coded datafile. If you don't pass in an SDK key, the Optimizely Client will not automatically sync newer versions of the datafile. Any time you retrieve an updated datafile, just re-instantiate the same client.

For simple applications, all you need to provide to instantiate a client is a [datafile](doc:get-the-datafile) specifying the project configuration for a given environment. For most advanced implementations, you'll want to [customize the logger](doc:customize-logger-ruby) or [error handler](doc:customize-error-handler-ruby) for your specific requirements.
[block:code]
{
  "codes": [
    {
      "code": "# Instantiate with both SDK key and datafile. The SDK will use the hard-coded datafile and will start polling for new datafiles remotely.\ndatafile = '{ revision: \"42\" }'\noptimizely_client = Optimizely::OptimizelyFactory.default_instance(sdk_key, datafile)\n\n# You can also customize the various components of the SDK (i.e. logger, error handler)\noptimizely_client = Optimizely::OptimizelyFactory.custom_instance(\n  sdk_key, \n  datafile\n  # event_dispatcher\n  # logger\n)\n\n# Prior to 3.2 you can instantiate with just a JSON datafile string\noptimizely_client = Optimizely::Project.new(datafile)\n\n",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Notes"
}
[/block]
### Enable JSON schema validation

Skipping JSON schema validation enhances performance during instantiation. In the Ruby SDK, you have control over whether to validate the JSON schema of the datafile when instantiating the client. This example shows how to skip JSON schema validation:
[block:code]
{
  "codes": [
    {
      "code": "# Skip JSON schema validation (SDK versions 0.1.1 and above)\noptimizely_client = Optimizely::Project.new(datafile, nil, nil, nil, true)\n\n",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Ruby are at [optimizely.rb](https://github.com/optimizely/ruby-sdk/blob/master/lib/optimizely.rb).