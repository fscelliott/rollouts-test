---
title: "Initialize SDK"
slug: "initialize-sdk-java"
hidden: false
createdAt: "2019-08-21T21:23:56.096Z"
updatedAt: "2019-08-21T21:24:14.097Z"
---
Use the builder to initialize the Java SDK and instantiate an instance of the Optimizely client class that exposes API methods like [Get Enabled Features](doc:get-enabled-features-java).
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

Some parameters are optional because the SDK provides a default implementation, but you may want to override these for your production environments. For example, you may want override these to set up an [error handler](doc:customize-error-handler-java) and [logger](doc:customize-logger-java) to catch issues, an event dispatcher to manage network calls, and a User Profile Service to ensure sticky bucketing.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
The table below lists the required and optional parameters in Java.
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
    "1-1": "ErrorHandler",
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
Use the builder to initialize the Java SDK and instantiate an instance of the Optimizely client class.
[block:code]
{
  "codes": [
    {
      "code": "<pre>\n    Optimizely optimizely = Optimizely.builder()\n        .withDatafile(datafile)\n        .withEventHandler(eventHandler)\n        .build();\n</pre>\n       \n       ",
      "language": "java"
    }
  ]
}
[/block]
For most advanced implementations, you'll want to [customize the logger](doc:customize-logger-java) or [error handler](doc:customize-error-handler-java) for your specific requirements.
[block:api-header]
{
  "title": "Exceptions"
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Exception",
    "h-1": "Meaning",
    "h-2": "Meaning",
    "0-0": "**ConfigParseException**",
    "0-1": "The datafile could not be parsed either because it is malformed or has an incorrect schema.",
    "0-2": "The datafile could not be parsed either because it is malformed or has an incorrect schema."
  },
  "cols": 2,
  "rows": 1
}
[/block]

[block:api-header]
{
  "title": "Notes"
}
[/block]
### Provide an event handler for Java

In addition to the datafile, you'll need to provide an event dispatcher (also called event handler) object as an argument to the `Optimizely.builder` function. Use our default event dispatcher implementation, or provide your own implementation as described in [Configure the event dispatcher](https://docs.developers.optimizely.com/full-stack/docs/configure-the-event-dispatcher).
[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Java are at [Optimizely.java](https://github.com/optimizely/java-sdk/blob/master/core-api/src/main/java/com/optimizely/ab/Optimizely.java).