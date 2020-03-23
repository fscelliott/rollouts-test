---
title: "Configure event dispatcher"
slug: "configure-event-dispatcher-php"
hidden: false
createdAt: "2019-09-12T15:59:33.605Z"
updatedAt: "2019-09-12T16:06:54.800Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. Each SDK has a built-in **event dispatcher** for handling these events, but we recommend overriding it based on the specifics of your environment.

The PHP SDK has an out-of-the-box synchronous dispatcher. We recommend customizing the event dispatcher you use in production to ensure that you queue and send events in a manner that scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.
[block:callout]
{
  "type": "warning",
  "body": "**Performance risks with synchronous dispatchers**: It's important to customize your event dispatcher when using an SDK for with a synchronous built-in event dispatcher (PHP, Ruby, and Python) to ensure that you can retrieve variations without waiting for the corresponding network request to return.",
  "title": "Important"
}
[/block]
The examples show that to customize the event dispatcher, initialize the Optimizely client (or manager) with an event dispatcher instance.
[block:code]
{
  "codes": [
    {
      "code": "use Optimizely\\Event\\Dispatcher\\DefaultEventDispatcher;\n\n/** \n * Create an Optimizely client with the default event dispatcher.\n * Please note, if not provided it will default to this event dispatcher.\n */\n$optimizelyClient = new Optimizely($datafile, new DefaultEventDispatcher());\n\n/**\n * You can provide your own implementation of the event dispatcher\n * for sending events to Optimizely. It should however implement\n * Optimizely\\Event\\Dispatcher\\EventDispatcherInterface.\n */\n\n",
      "language": "php"
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

Our PHP SDK includes a [basic synchronous event dispatcher](https://github.com/optimizely/php-sdk/blob/master/src/Optimizely/Event/Dispatcher/DefaultEventDispatcher.php).