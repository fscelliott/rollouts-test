---
title: "Initialize SDK"
slug: "initialize-sdk-javascript-node"
hidden: false
createdAt: "2019-08-21T21:38:02.209Z"
updatedAt: "2019-08-22T21:21:43.436Z"
---
Use the `createInstance` method to initialize the JavaScript (Node) SDK and instantiate an instance of the Optimizely client class that exposes API methods like [Get Enabled Features](doc:get-enabled-features-javascript-node). Each client corresponds to the datafile representing the state of a project for a certain environment.
[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.2.1
[block:api-header]
{
  "title": "Description"
}
[/block]
The `createInstance` method accepts a configuration object to configure Optimizely.

Some parameters are optional because the SDK provides a default implementation, but you may want to override these for your production environments. For example, you may want override these to set up an [error handler](doc:customize-error-handler-javascript-node) and [logger](doc:customize-logger-javascript-node) to catch issues, an event dispatcher to manage network calls, and a User Profile Service to ensure sticky bucketing.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional properties of the config object:
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**datafile**\n*optional* ",
    "0-1": "string",
    "0-2": "The JSON string representing the project. At least one of sdkKey or datafile must be provided.",
    "2-0": "**eventDispatcher**\n*optional*",
    "2-1": "object",
    "2-2": "An event dispatcher to manage network calls. An object with a dispatchEvent method.",
    "3-0": "**logger**\n*optional* ",
    "3-1": "object",
    "3-2": "A logger implementation to log messages. An object with a log method.",
    "4-0": "**errorHandler**\n*optional*",
    "4-1": "object",
    "4-2": "An error handler object to handle errors. An object with a handleError method",
    "5-0": "**userProfileService**\n*optional*",
    "5-1": "object",
    "5-2": "A user profile service. An object with lookup and save methods.",
    "6-0": "**skipJsonValidation**\n*optional*",
    "6-1": "Boolean",
    "6-2": "Specifies whether the JSON should be validated. Set to `true` to skip JSON validation on the schema, or `false` to perform validation.",
    "1-0": "**sdkKey**\n*optional*",
    "1-1": "string",
    "1-2": "The key associated with an environment in the project. At least one of sdkKey or datafile must be provided.",
    "7-0": "**datafileOptions**\n*optional*",
    "7-1": "object",
    "7-2": "Object with configuration for automatic datafile management. Can have `autoUpdate` (boolean), `urlTemplate` (string), and `updateInterval` (number) properties."
  },
  "cols": 3,
  "rows": 8
}
[/block]

[block:api-header]
{
  "title": "Returns"
}
[/block]
An instance of the Optimzely class.
[block:api-header]
{
  "title": "Examples"
}
[/block]
In the Node SDK, you can provide either `sdkKey` or `datafile` or both.

* When initializing with just the SDK key, the SDK will poll for datafile changes in the background at regular intervals.
* When initializing with just the datafile, the SDK will NOT poll for datafile changes in the background.
* When initializing with both the SDK key and datafile, the SDK will use the given datafile and start polling for datafile changes in the background.

### Instantiate using SDK Key

In the Node SDK, you only need to pass the SDK key value to instantiate a client. Whenever the experiment configuration changes, the SDK handles the change for you.

Include `sdkKey` as a string property in the options object you pass to the `createInstance` method.
[block:code]
{
  "codes": [
    {
      "code": "const optimizely = require('@optimizely/optimizely-sdk');\nconst optimizelyClientInstance = optimizely.createInstance({\n  sdkKey: '12345', // Provide the sdkKey of your desired environment here\n});\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]
When you provide the `sdkKey`, the SDK instance downloads the datafile associated with that `sdkKey`. When the download completes, the SDK instance updates itself to use the downloaded datafile. You can use the `onReady` method to wait for the datafile to be downloaded before using the instance.
[block:code]
{
  "codes": [
    {
      "code": "const optimizely = require('@optimizely/optimizely-sdk');\nconst optimizelyClientInstance = optimizely.createInstance({\n  sdkKey: '12345', // Provide the sdkKey of your desired environment here\n});\noptimizelyClientInstance.onReady().then(() => {\n  // optimizelyClientInstance is ready to use, with datafile downloaded from the\n  // Optimizely CDN\n});\n",
      "language": "javascript"
    }
  ]
}
[/block]
### Instantiate using datafile

To instantiate using datafile, first you must get a copy of the datafile from our server. In the example below, we demonstrate fetching the datafile using the request-promise library.
[block:code]
{
  "codes": [
    {
      "code": "// Minimal client\nconst optimizely = require(\"@optimizely/optimizely-sdk\");\nconst rp = require(\"request-promise\");\n\n// TODO: replace with your datafile URL\nconst DATAFILE_URL =\n  \"https://cdn.optimizely.com/datafiles/QMVJcUKEJZFg8pQ2jhAybK.json\";\nconst datafile = await rp({ uri: DATAFILE_URL, json: true });\nconsole.log(\"Datafile:\", datafile);\nconst optimizelyClient = optimizely.createInstance({\n  datafile\n});\n// Use optimizelyClient to run experiments\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]
If you don't pass in an SDK key, the Optimizely Client will not automatically sync newer versions of the datafile. Any time you retrieve an updated datafile, just re-instantiate the same client.

For simple applications, all you need to provide to instantiate a client is a [datafile](doc:get-the-datafile) specifying the project configuration for a given environment. For most advanced implementations, you'll want to [customize the logger](doc:customize-logger-javascript-node) or [error handler](doc:customize-error-handler-javascript-node) for your specific requirements.
[block:api-header]
{
  "title": "Notes"
}
[/block]
### Customize datafile management behavior

To customize datafile management behavior, provide a `datafileOptions` object property inside the `options` object passed to `createInstance`. The table lists the supported customizable options.
[block:parameters]
{
  "data": {
    "h-0": "Option",
    "h-1": "Type",
    "h-2": "Description",
    "0-2": "When true, and `sdkKey` was provided in `createInstance` options, automatic updates are enabled on this instance. The default value is `true`.",
    "1-2": "When automatic updates are enabled, this controls the update interval. The unit is milliseconds. The minimum allowed value is 1000 (1 second). The default value is 300000 milliseconds (5 minutes).",
    "2-2": "A format string used to build the URL from which the SDK will request datafiles. Instances of `%s` will be replaced with the `sdkKey`. When not provided, the SDK will request datafiles from the Optimizely CDN.",
    "0-1": "boolean",
    "1-1": "number",
    "2-1": "string",
    "0-0": "**autoUpdate**",
    "1-0": "**updateInterval**",
    "2-0": "**urlTemplate**"
  },
  "cols": 3,
  "rows": 3
}
[/block]
The following example shows how to customize datafile management behavior:
[block:code]
{
  "codes": [
    {
      "code": "const optimizely = require('@optimizely/optimizely-sdk');\n\nconst optimizelyClientInstance = optimizely.createInstance({\n  sdkKey: '12345',\n  datafileOptions: {\n    autoUpdate: true,\n    updateInterval: 600000 // 10 minutes in milliseconds\n    urlTemplate: 'http://localhost:5000/datafiles/%s.json',\n  },\n});",
      "language": "javascript"
    }
  ]
}
[/block]
### onReady

Use the onReady method to wait until the download is complete and the SDK is ready to use.

The onReady method returns a Promise representing the initialization process.
onReady accepts an optional timeout argument (defined in milliseconds) that controls the maximum duration that the returned Promise will remain in the pending state. If timeout is not provided, it defaults to 30 seconds.
[block:code]
{
  "codes": [
    {
      "code": "const optimizely = require('@optimizely/optimizely-sdk');\nlet instance = optimizely.createInstance({\n  sdkKey: '12345',\n});\ninstance.onReady().then(() => {\n  // optimizelyClientInstance is ready to use, with datafile downloaded from the Optimizely CDN\n});\n\n// Provide a timeout in milliseconds - promise will resolve if the datafile still is not available after the timeout\ninstance.onReady({ timeout: 5000 }).then(result => {\n  // Returned Promise is fulfilled with a result object\n  console.log(result.success); // true if the instance fetched a datafile and is now ready to use\n  console.log(result.reason); // If success is false, reason contains an error message\n})\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
The Promise returned from the onReady method is fulfilled with a result object containing a boolean success property.

When the `success` property is `true, the instance is ready to use with a valid datafile. When the `success property is `false`, there is also a `reason` string property describing the failure. Failure can be caused by expiration of the timeout, network error, unsuccessful HTTP response, datafile validation error, or the instance's `close` method being called.

### Set a fallback datafile

If you provide an sdkKey and a static fallback datafile for initialization, the SDK uses the fallback datafile immediately if it is valid while simultaneously downloading the datafile associated with the sdkKey. After the download completes, if the downloaded datafile is valid and has a more recent revision than the fallback datafile, the SDK updates the instance to use the downloaded datafile.

[block:code]
{
  "codes": [
    {
      "code": "const optimizely = require('@optimizely/optimizely-sdk');\nconst datafile = '{\"version\": \"4\", \"rollouts\": [], \"typedAudiences\": [], \"anonymizeIP\": false, \"projectId\": \"12345\", \"variables\": [], \"featureFlags\": [], \"experiments\": [], \"audiences\": [], \"groups\": [], \"attributes\": [], \"botFiltering\": false, \"accountId\": \"12345\", \"events\": [], \"revision\": \"1\"}'; \nconst optimizelyClientInstance = optimizely.createInstance({\n  sdkKey: '12345',\n  datafile,\n});\n// optimizelyClientInstance can be used immediately with the given datafile, but\n// will download the latest datafile and update itself",
      "language": "javascript"
    }
  ]
}
[/block]
### Enable JSON schema validation

Skipping JSON schema validation enhances performance during instantiation. In the JavaScript SDK, you have control over whether to validate the JSON schema of the datafile when instantiating the client. This example shows how to skip JSON schema validation:
[block:code]
{
  "codes": [
    {
      "code": "// Skip JSON schema validation for the datafile\nvar optimizelyClientInstance = optimizely.createInstance({\n  datafile: datafile,\n  skipJSONValidation: true\n});\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]
To enable JSON schema validation in JavaScript, you must pass in `false` for the `skip JSON validation` parameter and provide your own validator. See the [default validator](https://github.com/optimizely/javascript-sdk/blob/master/packages/optimizely-sdk/lib/utils/json_schema_validator/index.js) for an example.
[block:api-header]
{
  "title": "Source files"
}
[/block]
The source code files containing the implementation for the JavaScript SDK are [in the javascript-sdk repository on github](https://github.com/optimizely/javascript-sdk/tree/master/packages/optimizely-sdk/lib/).