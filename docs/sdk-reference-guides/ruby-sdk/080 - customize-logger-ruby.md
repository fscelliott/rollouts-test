---
title: "Customize logger"
slug: "customize-logger-ruby"
hidden: false
createdAt: "2019-08-21T21:52:52.045Z"
updatedAt: "2019-08-21T21:53:01.158Z"
---
The **logger** logs information about your experiments to help you with debugging. You can customize where log information is sent and what kind of information is tracked.

The Ruby SDK does not have a logger enabled by default. You need to pass in a logger to the SDK to have basic logging functionality. The Ruby SDK provides a simple logger implementation that you can instantiate and pass in. 

To improve your experience setting up the SDK and configuring your production environment, we recommend that you pass in a logger for your Optimizely client. See the code example below. 
[block:code]
{
  "codes": [
    {
      "code": "# Require the logger library in to get the log levels\nrequire 'logger'\n\n# Instantiate an Optimizely logger with the default log level Logger::INFO\nlogger = Optimizely::SimpleLogger.new \n\n# You can also instantiate with a custom log level\nlogger = Optimizely::SimpleLogger.new(Logger::DEBUG)\n\n# Instantiate the Optimizely client with our logger\noptimizely_client = Optimizely::Project.new(\n  datafile,\n  nil, # don't override the EventDispatcher\n  logger\n)\n\n",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Use your own logger"
}
[/block]
For finer control over your SDK configuration in a production environment, pass in a custom logger for your Optimizely client. A custom logger is a function that takes an argument, the level, and the message. See the code example below to create and set a custom logger.
[block:code]
{
  "codes": [
    {
      "code": "# Define your own custom logger that inherits from the Optimizely base logger\nclass CustomLogger < Optimizely::BaseLogger\n\n  def initialize()\n    @logger = Logger.new(STDOUT)\n  end\n\n  def log(level, message)\n    # You can handle the message here in any way you'd like\n    custom_message = \"Custom logged: #{message}\"\n    @logger.add(level, custom_message)\n  end\n\nend\n\n",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Log levels"
}
[/block]
The table below lists the log levels for the Ruby SDK.
[block:parameters]
{
  "data": {
    "0-0": "**Optimizely::SimpleLogger.new(Logger::CRITICAL)**",
    "h-0": "Log Level",
    "h-1": "Explanation",
    "0-1": "Events that cause the app to crash are logged.",
    "1-0": "**Optimizely::SimpleLogger.new(Logger::ERROR)**",
    "1-1": "Events that prevent feature flags from functioning correctly (for example, invalid datafile in initialization and invalid feature keys) are logged. The user can take action to correct.",
    "2-0": "**Optimizely::SimpleLogger.new(Logger::WARN)**",
    "2-1": "Events that don't prevent feature flags from functioning correctly, but can have unexpected outcomes (for example, future API deprecation, logger or error handler are not set properly, and nil values from getters) are logged.",
    "3-0": "**Optimizely::SimpleLogger.new(Logger::INFO)**",
    "3-1": "Events of significance (for example, activate started, activate succeeded, tracking started, and tracking succeeded) are logged. This is helpful in showing the lifecycle of an API call.",
    "4-1": "Any information related to errors that can help us debug the issue (for example, the feature flag is not running, user is not included in the rollout) are logged.",
    "4-0": "**Optimizely::SimpleLogger.new(Logger::DEBUG)**"
  },
  "cols": 2,
  "rows": 5
}
[/block]