---
title: "Configure event dispatcher"
slug: "configure-event-dispatcher-ruby"
hidden: true
createdAt: "2019-09-12T14:31:00.700Z"
updatedAt: "2019-09-12T16:44:17.172Z"
---
The Optimizely SDKs make HTTP requests for every impression or conversion that gets triggered. Each SDK has a built-in **event dispatcher** for handling these events, but we recommend overriding it based on the specifics of your environment.

The Ruby SDK has an out-of-the-box synchronous dispatcher. We recommend customizing the event dispatcher you use in production to ensure that you queue and send events in a manner that scales to the volumes handled by your application. Customizing the event dispatcher allows you to take advantage of features like batching, which makes it easier to handle large event volumes efficiently or to implement retry logic when a request fails. You can build your dispatcher from scratch or start with the provided dispatcher.
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
      "code": "# Create an Optimizely client with the default event dispatcher\noptimizely_client = Optimizely::Project.new(datafile,\n                                            Optimizely::EventDispatcher.new)\n\n",
      "language": "ruby"
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

Our Ruby SDK includes a [basic synchronous event dispatcher](https://github.com/optimizely/ruby-sdk/blob/master/lib/optimizely/event_dispatcher.rb). If you are using the SDK in a production environment, you should provide your own event dispatcher using the networking library of your choice. Examples of alternative approaches are available here:
* [SDK Wrapper: Cached Config by Ankit G](https://github.com/ankit8898/optimizely_server_side)