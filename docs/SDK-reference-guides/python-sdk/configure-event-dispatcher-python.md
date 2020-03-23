---
title: "Configure event dispatcher"
slug: "configure-event-dispatcher-python"
hidden: false
createdAt: "2019-09-12T16:20:19.224Z"
updatedAt: "2019-09-12T16:22:06.656Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. Each SDK has a built-in **event dispatcher** for handling these events, but we recommend overriding it based on the specifics of your environment.

The Python SDK has an out-of-the-box synchronous dispatcher. We recommend customizing the event dispatcher you use in production to ensure that you queue and send events in a manner that scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "**Performance risks with synchronous dispatchers**: It's important to customize your event dispatcher when using an SDK for with a synchronous built-in event dispatcher (PHP, Ruby, and Python) to ensure that you can retrieve variations without waiting for the corresponding network request to return."
}
[/block]
The examples show that to customize the event dispatcher, initialize the Optimizely client (or manager) with an event dispatcher instance.
[block:code]
{
  "codes": [
    {
      "code": "from optimizely.event_dispatcher import EventDispatcher as event_dispatcher\nfrom optimizely import optimizely\n\n# Create an Optimizely client with the default event dispatcher\noptimizely_client = optimizely.Optimizely(datafile,\n                                          event_dispatcher=event_dispatcher)\n\n",
      "language": "python"
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

Our Python SDK includes a [basic synchronous event dispatcher](https://github.com/optimizely/python-sdk/blob/master/optimizely/event_dispatcher.py). 
Examples of alternative approaches are available here:
  * [SDK Wrapper: Async Event Dispatcher by Cooper R](https://gist.github.com/cooperreid-optimizely/4d57682b39deb3557d437ae79c991eb3)
  * [SDK Wrapper: Bulk Event Dispatcher with AWS SQS by Matt A](https://github.com/mauerbac/opti_fullstack_event_dispatcher)