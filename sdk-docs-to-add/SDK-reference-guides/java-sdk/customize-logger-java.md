---
title: "Customize logger"
slug: "customize-logger-java"
hidden: false
createdAt: "2019-08-21T21:26:25.863Z"
updatedAt: "2019-08-21T21:26:49.030Z"
---
The **logger** logs information about your experiments to help you with debugging. You can customize where log information is sent and what kind of information is tracked.

In the Java SDK, logging functionality is not enabled by default. To improve your experience setting up the SDK and configuring your production environment, we recommend that you include a [SLF4J](https://www.slf4j.org/) implementation in your dependencies. For the Java SDK, we require the use of an [SLF4J](http://www.slf4j.org) implementation.

To improve your experience setting up the SDK and configuring your production environment, we recommend that you pass in a logger for your Optimizely client. See the code example below. 
[block:code]
{
  "codes": [
    {
      "code": "// Provide a SLF4j binding like logback or log4j\n\ncompile group: 'org.slf4j', name: 'slf4j-log4j12', version: '1.7.25'\ncompile group: 'log4j', name: 'log4j', version: '1.2.17'\n\n//For formatting: under slf4j.properties\n  \n# Root logger option\nlog4j.rootLogger=DEBUG, stdout\n\n# Redirect log messages to console\nlog4j.appender.stdout=org.apache.log4j.ConsoleAppender\nlog4j.appender.stdout.Target=System.out\nlog4j.appender.stdout.layout=org.apache.log4j.PatternLayout\nlog4j.appender.stdout.layout.ConversionPattern=%d\n\n{yyyy-MM-dd HH:mm:ss}\n%-5p %c\n\n{1}\n:%L - %m%n\n  \n  ",
      "language": "java"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Log levels"
}
[/block]
The table below lists the log levels for the Java SDK.
[block:parameters]
{
  "data": {
    "h-0": "Log Level",
    "h-1": "Explanation",
    "0-0": "**ERROR**",
    "0-1": "Events that prevent feature flags from functioning correctly (for example, invalid datafile in initialization and invalid feature keys) are logged. The user can take action to correct.",
    "1-0": "**WARNING**",
    "1-1": "Events that don't prevent feature flags from functioning correctly, but can have unexpected outcomes (for example, future API deprecation, logger or error handler are not set properly, and nil values from getters) are logged.",
    "2-0": "**INFO**",
    "2-1": "Events of significance (for example, activate started, activate succeeded, tracking started, and tracking succeeded) are logged. This is helpful in showing the lifecycle of an API call.",
    "3-1": "Any information related to errors that can help us debug the issue (for example, the feature flag is not running, user is not included in the rollout) are logged.",
    "3-0": "**DEBUG**"
  },
  "cols": 2,
  "rows": 4
}
[/block]