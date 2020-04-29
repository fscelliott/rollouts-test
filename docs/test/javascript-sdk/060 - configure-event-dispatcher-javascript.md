---
title: "Configure event dispatcher"
slug: "configure-event-dispatcher-javascript"
hidden: true
createdAt: "2019-09-12T14:35:43.588Z"
updatedAt: "2020-02-14T16:46:56.936Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. Each SDK has a built-in **event dispatcher** for handling these events, but we recommend overriding it based on the specifics of your environment.

The JavaScript SDK has an out-of-the-box asynchronous dispatcher. We recommend customizing the event dispatcher you use in production to ensure that you queue and send events in a manner that scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.

The examples show that to customize the event dispatcher, initialize the Optimizely client (or manager) with an event dispatcher instance. The JavaScript example is for the 3.0.0 and 3.0.1 SDKs.
[block:code]
{
  "codes": [
    {
      "code": "// 3.0.0 SDK\nvar defaultEventDispatcher = require('@optimizely/optimizely-sdk/lib/plugins/event_dispatcher/index.browser');\n\n// 3.0.1 SDK and above\nvar defaultEventDispatcher = require('@optimizely/optimizely-sdk').eventDispatcher;\n\n// Create an Optimizely client with the default event dispatcher\nvar optimizelyClientInstance = optimizely.createInstance({\n  datafile: datafile,\n  eventDispatcher: defaultEventDispatcher\n});\n\n",
      "language": "javascript"
    }
  ]
}
[/block]
The event dispatcher should implement a `dispatchEvent` function, which takes in two arguments: an object with `httpVerb`, `url`, and `params` properties, all of which are created by the internal `EventBuilder` class, and an optional callback. In this function, you should send a `POST` request to the given `url` using the `params` as the body of the request (be sure to stringify it to JSON) and `{content-type: 'application/json'}` in the headers.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "If you are using a custom event dispatcher, do not modify the event payload returned from Optimizely. Modifying this payload will alter your results."
}
[/block]
By default, our JavaScript SDK uses a [basic asynchronous event dispatcher](https://github.com/optimizely/javascript-sdk/blob/v3.4.1/packages/optimizely-sdk/lib/plugins/event_dispatcher/index.browser.js).