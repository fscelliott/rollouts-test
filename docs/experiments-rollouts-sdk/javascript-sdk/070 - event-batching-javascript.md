---
title: "Event batching"
slug: "event-batching-javascript"
hidden: true
createdAt: "2019-09-12T16:29:43.334Z"
updatedAt: "2019-12-13T00:32:31.922Z"
---
The [Optimizely Full Stack Javascript SDK](https://github.com/optimizely/javascript-sdk) lets you batch events and includes options to set a maximum batch size and flush interval timeout. The benefit of event batching means less network traffic for the same number of Impression and conversion events tracked.

The event batching functionality works by processing impression events from [Activate](doc:activate-javascript) and conversion events from [Track](doc:track-javascript) placing them into a queue. The queue is drained when it either reaches its maximum size limit or when the flush interval is triggered.

By default, event batching is enabled in JavaScript SDK 3.3.0.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Event batching works with both out-of-the-box and custom event dispatchers.\n\nThe event batching process doesn't remove any personally identifiable information (PII) from events. You must still ensure that you aren't sending any unnecessary PII to Optimizely."
}
[/block]

[block:api-header]
{
  "type": "basic",
  "title": "Basic example"
}
[/block]
We provide two options to configure event batching: `eventBatchSize` and `eventFlushInterval`. You can pass in both options during client creation. Events are held in a queue until either: 

* The number of events reaches the defined `eventBatchSize`.

* The oldest event has been in the queue for longer than the defined `eventFlushInterval`, which is specified in milliseconds. The queue is then flushed and all queued events are sent to Optimizely in a single network request.

* A new datafile revision is received. This occurs only when live datafile updates are enabled.

[block:code]
{
  "codes": [
    {
      "code": "let optimizelySdk = require('@optimizely/optimizely-sdk');\n\noptimizelySdk.createInstance({\n  // other options\n  eventBatchSize: 100,\n  eventFlushInterval: 3000,\n})\n",
      "language": "javascript"
    }
  ]
}
[/block]
By default, batch size is 10 and flush interval is 30 seconds.

The table below defines these options and lists general recommendations. On the browser we recommend using a small `eventBatchSize` (10) and a short `eventFlushInterval` (1000). This ensures that events are sent in a relatively fast manner, since some events could be lost if a user immediately bounces, while gaining network efficiency in cases where many decisions or [Track](doc:track-javascript) calls happen back-to-back.
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Description",
    "h-2": "Recommend Value",
    "0-0": "**eventBatchSize**",
    "1-0": "**eventFlushInterval**",
    "0-2": "10",
    "1-2": "100",
    "0-1": "Maximum size of batch of events to send to Optimizely.",
    "1-1": "The maximum duration in milliseconds that an event can exist in the queue before being flushed.\nDefault: 30000"
  },
  "cols": 3,
  "rows": 2
}
[/block]
For more information, see [Initialize SDK](doc:initialize-sdk-javascript).
[block:api-header]
{
  "title": "Side effects"
}
[/block]
The table lists other Optimizely functionality that may be triggered by using this class.
[block:parameters]
{
  "data": {
    "h-0": "Functionality",
    "h-1": "Description",
    "0-0": "**LogEvent**",
    "0-1": "Whenever the event processor produces a batch of events to be dispatched, a LogEvent notification object is created which contains\nbatch of conversion and impression events. \nThis object will be dispatched using the provided event dispatcher and also it will be sent to the notification subscribers",
    "1-0": "**Notification Listeners**",
    "1-1": "`flush()` invokes the LOG_EVENT [notification listener](doc:set-up-notification-listener-java) if this listener is subscribed."
  },
  "cols": 2,
  "rows": 2
}
[/block]
### Registering LogEvent listener

To register a LogEvent notification listener
[block:code]
{
  "codes": [
    {
      "code": "// Using notificationCenter to register logEvent listener\n\noptlimizely.notificationCenter\n  .addNotificationListener(optimizelyEnums.NOTIFICATION_TYPES.LOG_EVENT, () => {});",
      "language": "javascript"
    }
  ]
}
[/block]
###  LogEvent

It represents the batch of impression and conversion events we send to the Optimizely backend.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Description",
    "0-0": "**url**",
    "1-0": "**httpVerb**",
    "2-0": "**params**",
    "1-1": "The HTTP verb to use when dispatching the log event. It can be Get or post.",
    "0-1": "URL to dispatch log event to.",
    "2-1": "It contains all the information regarding every event which is batched."
  },
  "cols": 2,
  "rows": 3
}
[/block]

[block:api-header]
{
  "title": "Close Optimizely on application exit"
}
[/block]
If you enable event batching, it's important that you call the Close method (`optimizely.close()`) prior to exiting. This ensures that queued events are flushed as soon as possible to avoid any data loss.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "Because the Optimizely client maintains a buffer of queued events, we recommend that you call `close()` on the Optimizely instance before shutting down your application or whenever dereferencing the instance."
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Method",
    "h-1": "Description",
    "0-0": "**close()** ",
    "0-1": "Stops all timers and flushes the event queue. This method will also stop any timers that are happening for the datafile manager."
  },
  "cols": 2,
  "rows": 1
}
[/block]