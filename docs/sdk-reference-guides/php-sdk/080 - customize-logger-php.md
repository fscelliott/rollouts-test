---
title: "Customize logger"
slug: "customize-logger-php"
hidden: false
createdAt: "2019-08-21T21:42:51.218Z"
updatedAt: "2019-08-21T21:42:58.157Z"
---
The **logger** logs information about your experiments to help you with debugging. You can customize where log information is sent and what kind of information is tracked.

The PHP SDK comes with a default [`Logger` implementation](https://github.com/optimizely/php-sdk/blob/master/src/Optimizely/Logger/DefaultLogger.php). To configure the log level threshold, you can pass the log level as an argument to the DefaultLogger constructor.
[block:code]
{
  "codes": [
    {
      "code": "use Monolog\\Logger;\nuse Optimizely\\Logger\\DefaultLogger;\n\n/**\n * To set a log level choose one of the following:\n * \n * INFO: Logger.INFO\n * DEBUG: Logger.DEBUG\n * WARNING: Logger.WARNING\n * ERROR: Logger.ERROR\n * CRITICAL: Logger.CRITICAL\n *\n * To define a different minimum logging level pass it in during initialization\n * The example below shows a minimum logging level of WARNING and outputs\n * to standard out\n */\n\n$optimizelyClient = new Optimizely($datafile, null, new\nDefaultLogger(Logger::WARNING, \"stdout\"));\n\n",
      "language": "php"
    }
  ]
}
[/block]
For finer control over your SDK configuration in a production environment, pass in a custom logger for your Optimizely client. A custom logger is an instance of a class that implements [`LoggerInterface`](https://github.com/optimizely/php-sdk/blob/3.1.0/src/Optimizely/Logger/LoggerInterface.php)
[block:api-header]
{
  "title": "Log levels"
}
[/block]
The table below lists the log levels for the PHP SDK.
[block:parameters]
{
  "data": {
    "0-0": "**CRITICAL**",
    "h-0": "Log Level",
    "h-1": "Explanation",
    "0-1": "Events that cause the app to crash are logged.",
    "1-0": "**ERROR**",
    "1-1": "Events that prevent feature flags from functioning correctly (for example, invalid datafile in initialization and invalid feature keys) are logged. The user can take action to correct.",
    "2-0": "**WARNING**",
    "2-1": "Events that don't prevent feature flags from functioning correctly, but can have unexpected outcomes (for example, future API deprecation, logger or error handler are not set properly) are logged.",
    "3-0": "**INFO**",
    "3-1": "Events of significance (for example, activate started, activate succeeded, tracking started, and tracking succeeded) are logged. This is helpful in showing the lifecycle of an API call.",
    "4-0": "**DEBUG**",
    "4-1": "Any information related to errors that can help us debug the issue (for example, the feature flag is not running, user is not included in the rollout) are logged."
  },
  "cols": 2,
  "rows": 5
}
[/block]