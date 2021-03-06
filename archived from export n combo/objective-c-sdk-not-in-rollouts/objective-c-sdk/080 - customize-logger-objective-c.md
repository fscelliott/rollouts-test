---
title: "Customize logger"
slug: "customize-logger-objective-c"
hidden: true
createdAt: "2019-09-12T15:56:50.727Z"
updatedAt: "2019-09-13T12:02:06.644Z"
---
The **logger** logs information about your experiments to help you with debugging. You can customize where log information is sent and what kind of information is tracked.

To improve your experience setting up the SDK and configuring your production environment, we recommend that you pass in a logger for your Optimizely client. See the code examples below. 
[block:code]
{
  "codes": [
    {
      "code": "CustomLogger *customLogger = [[CustomLogger alloc] init];\n\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n  builder.sdkKey = @\"SDK_KEY_HERE\";\n  builder.logger = customLogger;\n}]];\n\nOPTLYLoggerDefault *optlyLogger = [[OPTLYLoggerDefault alloc] initWithLogLevel:OptimizelyLogLevelAll];\nOPTLYManager *manager = [[OPTLYManager alloc] initWithBuilder:[OPTLYManagerBuilder  builderWithBlock:^(OPTLYManagerBuilder * _Nullable builder) {\n  builder.sdkKey = @\"SDK_KEY_HERE\";\n  builder.logger = optlyLogger;\n}]];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let customLogger: CustomLogger? = CustomLogger()\n\nlet manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder?.sdkKey = \"SDK_KEY_HERE\"\n  builder?.logger = customLogger\n}))\n\nlet optlyLogger: OPTLYLoggerDefault? = OPTLYLoggerDefault(logLevel: OptimizelyLogLevel.all)\n\nlet manager = OPTLYManager(builder: OPTLYManagerBuilder(block: { (builder) in\n  builder?.sdkKey = \"SDK_KEY_HERE\"\n  builder?.logger = optlyLogger\n}))\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Log levels"
}
[/block]
The log levels vary slightly among SDKs but are generally as follows:
[block:parameters]
{
  "data": {
    "0-0": "**CRITICAL**",
    "h-0": "Log Level",
    "h-1": "Explanation",
    "0-1": "Events that cause the app to crash are logged.",
    "1-0": "**ERROR**",
    "1-1": "Events that prevent experiments from functioning correctly (for example, invalid datafile in initialization and invalid experiment keys or event keys) are logged. The user can take action to correct.",
    "2-0": "**WARNING**",
    "2-1": "Events that don't prevent experiments from functioning correctly, but can have unexpected outcomes (for example, future API deprecation, logger or error handler are not set properly, and nil values from getters) are logged.",
    "3-0": "**INFO**",
    "3-1": "Events of significance (for example, activate started, activate succeeded, tracking started, and tracking succeeded) are logged. This is helpful in showing the lifecycle of an API call.",
    "4-0": "**DEBUG**",
    "4-1": "Any information related to errors that can help us debug the issue (for example, experiment is not running, user is not in experiment, and the bucket value generated by our helper method) are logged."
  },
  "cols": 2,
  "rows": 5
}
[/block]

[block:api-header]
{
  "title": "Default logging"
}
[/block]
You can use our default logger and configure `OptimizelyLogLevelInfo`. You can initialize it with any log level and pass it in when creating an `Optimizely` instance. You can also implement your own logger and pass it in the builder when creating an `Optimizely` instance.