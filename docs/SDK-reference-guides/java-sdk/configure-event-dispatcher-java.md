---
title: "Configure event dispatcher"
slug: "configure-event-dispatcher-java"
hidden: false
createdAt: "2019-09-12T14:16:37.401Z"
updatedAt: "2019-09-12T14:26:31.437Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. Each SDK has a built-in **event dispatcher** for handling these events, but we recommend overriding it based on the specifics of your environment.

The Java SDK has an out-of-the-box asynchronous dispatcher. We recommend customizing the event dispatcher you use in production to ensure that you queue and send events in a manner that scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.

The examples show that to customize the event dispatcher, initialize the Optimizely client (or manager) with an event dispatcher instance.
[block:code]
{
  "codes": [
    {
      "code": "import com.optimizely.ab.Optimizely;\nimport com.optimizely.ab.config.parser.ConfigParseException;\nimport com.optimizely.ab.event.AsyncEventHandler;\nimport com.optimizely.ab.event.EventHandler;\n\n/**\n * Creates an async event handler with a max buffer of\n *  20,000 events and a single dispatcher thread\n */\nEventHandler eventHandler = new AsyncEventHandler(20000, 1);\n\nOptimizely optimizelyClient;\ntry {\n    // Create an Optimizely client with the default event dispatcher\n    optimizelyClient = Optimizely.builder(datafile,\n                                          eventHandler).build();\n} catch (ConfigParseException e) {\n    // Handle exception gracefully\n    return;\n}\n\n",
      "language": "java"
    }
  ]
}
[/block]
The event dispatcher should implement a `dispatchEvent` function, which takes in three arguments: `httpVerb`, `url`, and `params`, all of which are created by the internal `EventBuilder` class. In this function, you should send a `POST` request to the given `url` using the `params` as the body of the request (be sure to stringify it to JSON) and `{content-type: 'application/json'}` in the headers.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "If you are using a custom event dispatcher, do not modify the event payload returned from Optimizely. Modifying this payload will alter your results."
}
[/block]
We provide an asynchronous event dispatcher, [optimizely-sdk-httpclient](https://bintray.com/optimizely/optimizely/optimizely-sdk-httpclient), that requires `org.apache.httpcomponents:httpclient:4.5.2` and is parameterized by in-memory queue size and number of dispatch worker threads.

To use your own event dispatcher, implement the `dispatchEvent` method in our [EventHandler](https://github.com/optimizely/java-sdk/blob/master/core-api/src/main/java/com/optimizely/ab/event/EventHandler.java) interface. `dispatchEvent` takes in a [LogEvent](https://github.com/optimizely/java-sdk/blob/master/core-api/src/main/java/com/optimizely/ab/event/LogEvent.java) object containing all of the information needed to dispatch an event to Optimizely's backend. Events should be sent to `LogEvent.getEndpointUrl` as a `POST` request with `LogEvent.getBody` as the payload and `content-type: "application/json"` as the header.