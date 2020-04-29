---
title: "Customize logger"
slug: "customize-logger-python"
hidden: true
createdAt: "2019-08-21T21:49:21.267Z"
updatedAt: "2019-08-21T21:49:28.633Z"
---
The **logger** logs information about your experiments to help you with debugging. You can customize where log information is sent and what kind of information is tracked.

You can use our [`SimpleLogger` implementation](https://github.com/optimizely/python-sdk/blob/master/optimizely/logger.py#L19) out of the box.

To improve your experience setting up the SDK and configuring your production environment, we recommend that you pass in a logger for your Optimizely client. See the code example below. 
[block:code]
{
  "codes": [
    {
      "code": "import logging\nfrom optimizely import logger\nfrom optimizely import optimizely\n\n# To set a log level choose one of the following:\n# INFO: logging.INFO\n# NOTSET: logging.NOTSET\n# DEBUG: logging.DEBUG\n# WARNING: logging.WARNING\n# ERROR: logging.ERROR\n# CRITICAL: logging.CRITICAL\n\n# To define a minimum logging level pass it in during initialization.\n# The example below shows a minimum logging level of INFO.\noptimizely_client = optimizely.Optimizely(datafile, logger=logger.SimpleLogger(min_level=logging.INFO))\n\n",
      "language": "python"
    }
  ]
}
[/block]
For finer control over your SDK configuration in a production environment, pass in a custom logger for your Optimizely client. A custom logger is a function that takes an argument, the level, and the message. See the code example below to create and set a custom logger.
[block:code]
{
  "codes": [
    {
      "code": "import logging\n\ndef get_custom_logger(name, level=logging.INFO):\n  \"\"\" Example of a custom logger.\n  \n    This function takes in two parameters: name and level and logs to console.\n    The place to log in this case is defined by the handler which we set\n    to logging.StreamHandler().\n    \n    Args:\n      name: Name for the logger.\n      level: Minimum level for messages to be logged\n  \"\"\"\n  logger = logging.getLogger(name)\n  logger.setLevel(level)\n\n  if not logger.handlers:\n    handler = logging.StreamHandler()\n    formatter = logging.Formatter(\"Optimizely({}): %(levelname)s %(message)s\".format(name))\n    handler.setFormatter(formatter)\n    logger.addHandler(handler)\n\n  return logger\n\n\n# Here we initialize the SDK with the custom logger.\noptimizely_client = optimizely.Optimizely(datafile, logger=get_custom_logger('my_optimizely_logger'))\n\n",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Log levels"
}
[/block]
The table below lists the log levels for the Python SDK.
[block:parameters]
{
  "data": {
    "h-0": "Log Level",
    "h-1": "Explanation",
    "0-0": "**logging.CRITICAL**",
    "0-1": "Events that cause the app to crash are logged.",
    "2-0": "**logging.WARNING**",
    "2-1": "Events that don't prevent feature flags from functioning correctly, but can have unexpected outcomes (for example, future API deprecation, logger or error handler are not set properly, and nil values from getters) are logged.",
    "3-0": "**logging.INFO**",
    "3-1": "Events of significance (for example, activate started, activate succeeded, tracking started, and tracking succeeded) are logged. This is helpful in showing the lifecycle of an API call.",
    "5-0": "**logging.NOTSET**",
    "5-1": "Level set when a logger is created. Causes all messages to be processed when the logger is the root logger (or delegation to the parent when the logger is a non-root logger).",
    "4-1": "Any information related to errors that can help us debug the issue (for example, the feature flag is not running, user is not included in the rollout) are logged.",
    "4-0": "**logging.DEBUG**",
    "1-1": "Events that prevent feature flags from functioning correctly (for example, invalid datafile in initialization and invalid feature keys) are logged. The user can take action to correct.",
    "1-0": "**logging.ERROR**"
  },
  "cols": 2,
  "rows": 6
}
[/block]