---
title: "Initialize SDK"
slug: "initialize-sdk-react"
hidden: false
createdAt: "2019-09-11T14:16:42.771Z"
updatedAt: "2019-10-08T18:20:57.948Z"
---
Use the `createInstance` method to initialize the React SDK and instantiate an instance of the ReactSDKClient class. Each client corresponds to the datafile representing the state of a project for a certain environment.
[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v1.0.0
[block:api-header]
{
  "title": "Description"
}
[/block]
The `createInstance` method accepts a configuration object to configure Optimizely.

Some parameters are optional because the SDK provides a default implementation, but you may want to override these for your production environments. For example, you may want override these to set up an [error handler](doc:customize-error-handler-react) and [logger](doc:customize-logger-react) to catch issues, an event dispatcher to manage network calls, and a User Profile Service to ensure sticky bucketing.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in React (created by passing in a Config object. The Config class consists of these fields).
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
    "2-2": "An event handler to manage network calls.",
    "3-0": "**logger**\n*optional* ",
    "3-1": "object",
    "3-2": "A logger implementation to log issues.",
    "4-0": "**errorHandler**\n*optional*",
    "4-1": "object",
    "4-2": "An error handler object to handle errors.",
    "5-0": "**userProfileService**\n*optional*",
    "5-1": "object",
    "5-2": "A user profile service.",
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
Instantiates an instance of the Optimzely class.
[block:api-header]
{
  "title": "Example"
}
[/block]
In the React SDK, you can provide either `sdkKey` or `datafile` or both.

* When initializing with just the SDK key, the datafile will be automatically downloaded
* When initializing with just the datafile, the SDK will use the given datafile.
* When initializing with both the SDK key and datafile, the SDK will use the given datafile to start, then download the latest version of the datafile in the background.

### Instantiate using datafile

First, get a copy of the datafile from our server by including the following script in the <head> of your web application. This script adds the datafile on the window variable `window.optimizelyDatafile`.
[block:code]
{
  "codes": [
    {
      "code": "<script src=\"https://cdn.optimizely.com/datafiles/<Your_SDK_Key>.json/tag.js\"></script>\n\n",
      "language": "html"
    }
  ]
}
[/block]
Once you have a datafile, you can instantiate an Optimizely client. 
[block:code]
{
  "codes": [
    {
      "code": "import * as optimizelyReactSDK from '@optimizely/react-sdk';\n\n// Instantiate an Optimizely client\nconst optimizely = optimizelyReactSDK.createInstance({\n  datafile: window.optimizelyDatafile,\n})\n\n",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]
### Instantiate using SDK Key

To instantiate using SDK Key, obtain the SDK Key from your project's settings page, then pass `sdkKey` as a string property in the options object you pass to the `createInstance` method.
[block:code]
{
  "codes": [
    {
      "code": "import * as optimizelyReactSDK from '@optimizely/react-sdk';\n\n// Instantiate an Optimizely client\nconst optimizely = optimizelyReactSDK.createInstance({\n  sdkKey: '<Your_SDK_Key>',\n});\n\n",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]
When you provide the `sdkKey`, the SDK instance downloads the datafile associated with that `sdkKey`. When the download completes, the SDK instance updates itself to use the downloaded datafile.

### Render an `OptimizelyProvider` with a client and user

To use React SDK components inside your app, render an `OptimizelyProvider` as the parent of your root app component. Provide your Optimizely instance as the `optimizely` prop, and a `user` object:
[block:code]
{
  "codes": [
    {
      "code": "import { OptimizelyProvider, createInstance  } from '@optimizely/react-sdk'\n\nconst optimizely = createInstance({\n  datafile: window.datafile,\n});\n\nclass AppWrapper extends React.Component {\n  render() {\n    return (\n      <OptimizelyProvider\n      \toptimizely={optimizely}\n  \t\t\tuser={{id: '<Your_User_Id>'}}>\n        <App />\n      </OptimizelyProvider>\n    );\n  }\n}\n\n",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]

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
    "0-0": "**autoUpdate**",
    "0-1": "boolean",
    "0-2": "When true, and `sdkKey` was provided in `createInstance` options, automatic updates are enabled on this instance. The default value is `false`.",
    "1-0": "**updateInterval**",
    "1-1": "number",
    "1-2": "When automatic updates are enabled, this controls the update interval. The unit is milliseconds. The minimum allowed value is 1000 (1 second). The default value is 300000 milliseconds (5 minutes).",
    "2-0": "**urlTemplate**",
    "2-1": "string",
    "2-2": "A format string used to build the URL from which the SDK will request datafiles. Instances of `%s` will be replaced with the `sdkKey`. When not provided, the SDK will request datafiles from the Optimizely CDN."
  },
  "cols": 3,
  "rows": 3
}
[/block]
The following example shows how to customize datafile management behavior.
[block:code]
{
  "codes": [
    {
      "code": "import { createInstance } from '@optimizely/react-sdk';\n\nconst optimizely = createInstance({\n  sdkKey: '<Your_SDK_Key>',\n  datafileOptions: {\n    autoUpdate: true,\n    updateInterval: 600000, // 10 minutes in milliseconds\n    urlTemplate: 'http://localhost:5000/datafiles/%s.json',\n  },\n});\n\n",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for React SDK are at [index.ts](https://github.com/optimizely/react-sdk/blob/master/src/index.ts).