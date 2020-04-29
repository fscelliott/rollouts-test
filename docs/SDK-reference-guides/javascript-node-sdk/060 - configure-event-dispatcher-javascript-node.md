---
title: "Configure event dispatcher"
slug: "configure-event-dispatcher-javascript-node"
hidden: true
createdAt: "2019-09-12T14:59:32.522Z"
updatedAt: "2019-09-12T15:01:20.137Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. Each SDK has a built-in **event dispatcher** for handling these events, but we recommend overriding it based on the specifics of your environment.

The Node SDK has an out-of-the-box asynchronous dispatcher. We recommend customizing the event dispatcher you use in production to ensure that you queue and send events in a manner that scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.

The examples show that to customize the event dispatcher, initialize the Optimizely client (or manager) with an event dispatcher instance. The Node example is for the 3.0.0 and 3.0.1 SDKs.
[block:code]
{
  "codes": [
    {
      "code": "// Client with defaultEventDispatcher\n// 3.0.0 SDK\nconst defaultEventDispatcher = require(\"@optimizely/optimizely-sdk/lib/plugins/event_dispatcher/index.node\");\n\n// 3.0.1 SDK and above\nconst defaultEventDispatcher = require('@optimizely/optimizely-sdk').eventDispatcher;\n\n// Create an Optimizely client with the default event dispatcher\nvar optimizelyClientWithDefaultDispatcher = optimizely.createInstance({\n  datafile,\n  eventDispatcher: defaultEventDispatcher\n});\n\n",
      "language": "javascript",
      "name": "Node"
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