---
title: "Initialize SDK"
slug: "initialize-sdk-python"
hidden: false
createdAt: "2019-08-21T21:48:57.310Z"
updatedAt: "2019-08-22T21:29:29.026Z"
---
Use the `instantiate` method to initialize the Python SDK and instantiate an instance of the Optimizely client class that exposes API methods like [Get Enabled Features](doc:get-enabled-features-python). Each client corresponds to the datafile representing the state of a project for a certain environment.
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

Some parameters are optional because the SDK provides a default implementation, but you may want to override these for your production environments. For example, you may want override these to set up an [error handler](doc:customize-error-handler-python) and [logger](doc:customize-logger-python) to catch issues, an event dispatcher to manage network calls, and a User Profile Service to ensure sticky bucketing.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in Python.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**datafile**\n*optional* ",
    "0-1": "string",
    "0-2": "The JSON string representing the project.\nMust provide either `datafile` or `sdk_key`.",
    "2-0": "**event_dispatcher**\n*optional*",
    "2-1": "EventDispatcher",
    "2-2": "An event handler to manage network calls.",
    "3-0": "**logger**\n*optional* ",
    "3-1": "logging.Logger",
    "3-2": "A logger implementation to log issues.",
    "4-0": "**error_handler**\n*optional*",
    "4-1": "BaseErrorHandler",
    "4-2": "An error handler object to handle errors.",
    "5-0": "**user_profile_service**\n*optional*",
    "5-1": "UserProfileService",
    "5-2": "A user profile service.",
    "6-0": "**skip_json_validation**\n*optional*",
    "6-1": "Boolean",
    "6-2": "Specifies whether the JSON should be validated. Set to `true` to skip JSON validation on the schema, or `false` to perform validation.",
    "7-0": "**config_manager**\n*optional*",
    "8-0": "**notification_center**\n*optional*",
    "7-2": "Implements `optimizely.config_manager.BaseConfigManager`.\nResponsible for providing `get_config` method which returns an instance of `optimizely.project_config.ProjectConfig`.",
    "1-0": "**sdk_key**\n*optional*",
    "1-1": "string",
    "1-2": "Optional string that uniquely identifies the datafile corresponding to project and environment combination.\nMust provide either `datafile` or `sdk_key`.",
    "8-2": "Instance of `optimizely.notification_center.NotificationCenter`.\nThis option is useful when providing your own `optimizely.config_manager.BaseConfigManager` implementation, which can use the same NotificationCenter instance.",
    "7-1": "BaseConfigManager",
    "8-1": "NotificationCenter"
  },
  "cols": 3,
  "rows": 9
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
In the Python SDK, you can provide either `sdkKey` or `datafile` or both.

* When initializing with just the SDK key, the SDK will poll for datafile changes in the background at regular intervals.
* When initializing with just the datafile, the SDK will NOT poll for datafile changes in the background.
* When initializing with both the SDK key and datafile, the SDK will use the given datafile and start polling for datafile changes in the background.

### Instantiate using SDK Key (recommended)

In the Python SDK, you only need to pass the SDK key value to instantiate a client. Whenever the experiment configuration changes, the SDK handles the change for you.

Include `sdkKey` as a string property in the options object you pass to the `createInstance` method.

When you provide the `sdkKey`, the SDK instance downloads the datafile associated with that `sdkKey`. When the download completes, the SDK instance updates itself to use the downloaded datafile.
[block:code]
{
  "codes": [
    {
      "code": "from optimizely import optimizely\nfrom optimizely.config_manager import PollingConfigManager\n \nsdk_key = ‘123456’\nconf_manager = PollingConfigManager(\n   sdk_key = sdk_key,\n   update_interval=10\n)\n \noptimizely_instance=optimizely.Optimizely(config_manager=conf_manager)\n\n",
      "language": "python"
    }
  ]
}
[/block]
### Instantiate using datafile

You can also instantiate with a hard-coded datafile. If you don't pass in an SDK key, the Optimizely Client will not automatically sync newer versions of the datafile. Any time you retrieve an updated datafile, just re-instantiate the same client.

For simple applications, all you need to provide to instantiate a client is a [datafile](doc:get-the-datafile) specifying the project configuration for a given environment. For most advanced implementations, you'll want to [customize the logger](doc:customize-logger-python) or [error handler](doc:customize-error-handler-python) for your specific requirements.
[block:code]
{
  "codes": [
    {
      "code": "from optimizely import optimizely\n\n# Instantiate an Optimizely client\noptimizely_client = optimizely.Optimizely(datafile)\n",
      "language": "python"
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

Skipping JSON schema validation enhances performance during instantiation. In the Python SDK, you have control over whether to validate the JSON schema of the datafile when instantiating the client. This example shows how to skip JSON schema validation:
[block:code]
{
  "codes": [
    {
      "code": "# Skip JSON schema validation (SDK versions 0.1.1 and above)\noptimizely_client = optimizely.Optimizely(datafile,\n                                          skip_json_validation=True)\n\n",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Python are at [optimizely.py](https://github.com/optimizely/python-sdk/blob/master/optimizely/optimizely.py).