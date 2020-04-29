---
title: "Customize logger"
slug: "customize-logger-java"
hidden: false
createdAt: "2019-08-21T21:26:25.863Z"
updatedAt: "2020-04-27T19:05:18.313Z"
---
The **logger** logs information about your experiments to help you with debugging. You can customize where log information is sent and what kind of information is tracked.

In the Java SDK, logging functionality is not enabled by default. To improve your experience setting up the SDK and configuring your production environment, we recommend that you include a [SLF4J](https://www.slf4j.org/) implementation in your dependencies. For the Java SDK, we require the use of an [SLF4J](http://www.slf4j.org) implementation.

To improve your experience setting up the SDK and configuring your production environment, we recommend that you pass in a logger for your Optimizely client. See the code example below. 
[block:code]
{
  "codes": [
    {
      "code": "// Provide a SLF4j binding like logback or log4j\n\ncompile group: 'org.slf4j', name: 'slf4j-log4j12', version: '1.7.25'\ncompile group: 'log4j', name: 'log4j', version: '1.2.17'\n",
      "language": "java",
      "name": "build.gradle"
    }
  ]
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "//For formatting: under slf4j.properties\n  \n# Root logger option\nlog4j.rootLogger=DEBUG, stdout\n\n# Redirect log messages to console\nlog4j.appender.stdout=org.apache.log4j.ConsoleAppender\nlog4j.appender.stdout.Target=System.out\nlog4j.appender.stdout.layout=org.apache.log4j.PatternLayout\nlog4j.appender.stdout.layout.ConversionPattern=%d\n\n{yyyy-MM-dd HH:mm:ss}\n%-5p %c\n\n{1}\n:%L - %m%n\n  \n  ",
      "language": "java",
      "name": "slf4j.properties"
    }
  ]
}
[/block]
For finer control over logging, for example to control the logging level per package or to control the logging destination, we recommend you use  [logback](http://logback.qos.ch/reasonsToSwitch.html).
[block:code]
{
  "codes": [
    {
      "code": "  implementation 'ch.qos.logback:logback-classic:1.1.7'",
      "language": "java",
      "name": "build.gradle"
    }
  ]
}
[/block]
Save the logback.xml file in your resources directory.  Below is a simple example of capturing Optimizely log levels and piping them somewhere else.
[block:code]
{
  "codes": [
    {
      "code": "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<configuration>\n\n    <appender name=\"CONSOLE\" class=\"ch.qos.logback.core.ConsoleAppender\">\n        <encoder class=\"ch.qos.logback.classic.encoder.PatternLayoutEncoder\">\n            <pattern>\n                %d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n\n            </pattern>\n        </encoder>\n    </appender>\n\n    <appender name=\"MEMORY\" class=\"com.optimizely.intellij.plugin.utils.LogAppender\">\n        <encoder class=\"ch.qos.logback.classic.encoder.PatternLayoutEncoder\">\n            <pattern>\n                %d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n\n            </pattern>\n        </encoder>\n    </appender>\n\n    <logger name=\"com.optimizely\" level=\"debug\" additivity=\"false\">\n        <appender-ref ref=\"MEMORY\"/>\n    </logger>\n\n    <root level=\"error\">\n        <appender-ref ref=\"CONSOLE\"/>\n    </root>\n",
      "language": "java",
      "name": "logback.xml"
    }
  ]
}
[/block]
The preceding example pipes all debug logs to error messages.  These are then sent to a different log appender. 

You can also set logging levels for any Optimizely package, giving you control over logs granularity. In the following example, we simply set the log level for the highest-level package, com.optimizely:
[block:code]
{
  "codes": [
    {
      "code": "<logger name=\"com.optimizely\" level=\"debug\" />",
      "language": "java",
      "name": "logback.xml"
    }
  ]
}
[/block]


Finally, the following general example show you how to write a custom appender rather than using one of the many default appenders:
[block:code]
{
  "codes": [
    {
      "code": "public class LogAppender extends AppenderBase<ILoggingEvent> {\n    int counter = 0;\n    public static AtomicBoolean captureLogging = new AtomicBoolean(false);\n    public static List<String> logs = Collections.synchronizedList(new ArrayList<String>());\n\n    PatternLayoutEncoder encoder;\n\n    @Override\n    public void start() {\n        if (this.encoder == null) {\n            addError(\"No encoder set for the appender named [\"+ name +\"].\");\n            return;\n        }\n\n        try {\n            encoder.init(System.out);\n        } catch (IOException e) {\n        }\n        super.start();\n    }\n\n    public static void clearLogs() {\n        synchronized (logs) {\n            logs.clear();\n        }\n    }\n\n    public void append(ILoggingEvent event) {\n        // output the events as formatted by our layout\n        try {\n            this.encoder.doEncode(event);\n            if (captureLogging.get()) {\n                synchronized (logs) {\n                    logs.add(event.getFormattedMessage());\n                }\n            }\n        } catch (IOException e) {\n        }\n\n        // prepare for next event\n        counter++;\n    }\n\n    public PatternLayoutEncoder getEncoder() {\n        return encoder;\n    }\n\n    public void setEncoder(PatternLayoutEncoder encoder) {\n        this.encoder = encoder;\n    }\n}",
      "language": "java",
      "name": "LogAppender example"
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