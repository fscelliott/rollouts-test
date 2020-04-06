---
title: "Customize logger"
slug: "customize-logger-react"
hidden: false
createdAt: "2019-09-12T15:13:39.389Z"
updatedAt: "2019-11-25T21:14:53.029Z"
---
The **logger** logs information about your experiments to help you with debugging. You can customize where log information is sent and what kind of information is tracked.

The React SDK comes with a default [`Logger` implementation](https://github.com/optimizely/javascript-sdk/blob/master/packages/optimizely-sdk/lib/plugins/logger/index.js). To configure the log level threshold, you can call `setLogLevel`.
[block:code]
{
  "codes": [
    {
      "code": "import {\n  setLogLevel,\n} from '@optimizely/react-sdk'\n\n// Set log level to debug\n// Can be 'info', 'debug', 'warn', 'error'\nsetLogLevel('debug'); ",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]
To turn off logging from the SDK, call `setLogger` with `null`:
[block:code]
{
  "codes": [
    {
      "code": "import {\n  setLogger,\n} from '@optimizely/react-sdk'\n\n// To turn off logging, call setLogger with null\nsetLogger(null);",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]
For finer control over your SDK configuration in a production environment, pass in a custom logger for your Optimizely client. A custom logger is a function that takes an argument, the level, and the message. See the code example below to create and set a custom logger.
[block:code]
{
  "codes": [
    {
      "code": "import {\n  setLogLevel,\n  setLogger,\n} from '@optimizely/react-sdk'\n\n// Set log level\nsetLogLevel('debug');\n\n/**\n * customLogger\n * \n * Example of a custom logger. A custom logger is a function that\n * takes two parameters (level, message) and logs to an appropriate place,\n * typically the console.\n *\n * @param {string} level indicating the level of the log message\n * @param {string} message indicating the log message\n *\n */\nvar customLogger = function(level, message) {\n  var LOG_LEVEL = optimizelySDK.enums.LOG_LEVEL;\n  switch (level) {\n    case LOG_LEVEL.INFO:\n      // INFO log message\n      console.log(message);\n      break;\n    \n    case LOG_LEVEL.DEBUG:\n      // DEBUG log message\n      console.log(message);\n      break;\n\n    case LOG_LEVEL.WARNING:\n      // WARNING log message\n      console.log(message);\n      break;\n\n    case LOG_LEVEL.ERROR:\n      // ERROR log message\n      console.log(message);\n      break;\n  }\n}\n\n// Set the custom logger\nsetLogger({\n  log: customLogger,\n});\n\n",
      "language": "javascript"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Log levels"
}
[/block]
The table below lists the log levels for the JavaScript (React) SDK.
[block:parameters]
{
  "data": {
    "h-0": "Log Level",
    "h-1": "Explanation",
    "0-0": "**ERROR**",
    "0-1": "Events that prevent feature flags from functioning correctly (for example, invalid datafile in initialization and invalid feature keys) are logged. The user can take action to correct.",
    "1-0": "**WARNING**",
    "1-1": "Events that don't prevent feature flags from functioning correctly, but can have unexpected outcomes (for example, future API deprecation, logger or error handler are not set properly) are logged.",
    "2-0": "**INFO**",
    "2-1": "Events of significance (for example, activate started, activate succeeded, tracking started, and tracking succeeded) are logged. This is helpful in showing the lifecycle of an API call.",
    "3-0": "**DEBUG**",
    "3-1": "Any information related to errors that can help us debug the issue (for example, the feature flag is not running, user is not included in the rollout) are logged."
  },
  "cols": 2,
  "rows": 4
}
[/block]