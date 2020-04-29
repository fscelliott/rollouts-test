---
title: "Initialize SDK"
slug: "initialize-sdk-php"
hidden: false
createdAt: "2019-08-21T21:41:58.070Z"
updatedAt: "2019-08-23T20:27:31.526Z"
---
Use the `instantiate` method to initialize the PHP SDK and instantiate an instance of the Optimizely client class that exposes API methods like [Get Enabled Features](doc:get-enabled-features-php). Each client corresponds to the datafile representing the state of a project for a certain environment.
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

Some parameters are optional because the SDK provides a default implementation, but you may want to override these for your production environments. For example, you may want override these to set up an [error handler](doc:customize-error-handler-php) and [logger](doc:customize-logger-php) to catch issues, an event dispatcher to manage network calls, and a User Profile Service to ensure sticky bucketing.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in PHP (client constructed using Builder class and passing to `Build()`).
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**datafile**\n*optional* ",
    "0-1": "string",
    "0-2": "The JSON string representing the project.",
    "1-0": "**errorHandler**\n*optional*",
    "1-1": "IErrorHandler",
    "1-2": "An error handler object to handle errors."
  },
  "cols": 3,
  "rows": 2
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
You can instantiate with a hard-coded datafile. If you don't pass in an SDK key, the Optimizely Client will not automatically sync newer versions of the datafile. Any time you retrieve an updated datafile, just re-instantiate the same client.

For simple applications, all you need to provide to instantiate a client is a [datafile](doc:get-the-datafile) specifying the project configuration for a given environment. For most advanced implementations, you'll want to [customize the logger](doc:customize-logger-php) or [error handler](doc:customize-error-handler-php) for your specific requirements.
[block:code]
{
  "codes": [
    {
      "code": "$template = \"https://cdn.optimizely.com/datafiles/%s.json\";\n\n$configManager = new HTTPProjectConfigManager('QBw9gFM8oTn7ogY9ANCC1z', null, $template, true, null, false, null, null);\n\n$optimizelyClient  = new Optimizely($datafile, null, null, null, false, null, $configManager);\n\n",
      "language": "php"
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

Skipping JSON schema validation enhances performance during instantiation. In the PHP SDK, you have control over whether to validate the JSON schema of the datafile when instantiating the client. This example shows how to skip JSON schema validation:
[block:code]
{
  "codes": [
    {
      "code": "// Skip JSON schema validation\n$optimizelyClient = new Optimizely($datafile);\n\n",
      "language": "php"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for PHP are at [Optimizely.php](https://github.com/optimizely/php-sdk/blob/master/src/Optimizely/Optimizely.php).