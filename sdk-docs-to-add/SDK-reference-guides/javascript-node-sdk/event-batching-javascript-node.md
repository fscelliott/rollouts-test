---
title: "Event batching"
slug: "event-batching-javascript-node"
hidden: false
createdAt: "2019-09-24T17:47:58.348Z"
updatedAt: "2019-10-11T16:49:53.886Z"
---
The [Optimizely Full Stack JavaScript SDK 3.3.0](https://github.com/optimizely/javascript-sdk/tree/v3.3.0-beta) lets you batch events and includes options to set a maximum batch size and flush interval timeout. The benefit of event batching means less network traffic for the same number of Impression and conversion events tracked. 

The event batching functionality works by processing impression events from [Activate](doc:activate-javascript-node) and conversion events from [Track](doc:track-javascript-node) placing them into a queue. The queue is drained when it either reaches its maximum size limit or when the flush interval is triggered.

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
  "title": "Configure event batching"
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
      "code": "const optimizelySdk = require('@optimizely/optimizely-sdk')\n\noptimizelySdk.createInstance({\n  // other options\n  eventBatchSize: 100,\n  eventFlushInterval: 3000,\n})\n\n",
      "language": "javascript",
      "name": ""
    }
  ]
}
[/block]
The table below defines these options and lists general recommendations for browser and server default settings. On the browser we recommend using a small `eventBatchSize` (10) and a short `eventFlushInterval` (1000). This ensures that events are sent in a relatively fast manner, since some events could be lost if a user immediately bounces, while gaining network efficiency in cases where many decisions or [Track](doc:track-javascript-node) calls happen back-to-back.

For the server, these numbers are more flexible and should be set based on your organization's traffic and needs. Please [contact support](https://help.optimizely.com/Account_Settings/File_online_tickets_for_support) for recommendations for your specific implementation.
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Description",
    "0-0": "**eventBatchSize** ",
    "0-1": "The maximum number of events to hold in the queue. Once this number is reached, all queued events are flushed and sent to Optimizely.\nDefault: 10\n\n**Note**: By setting this value to 1, events aren't batched and event requests behave exactly as in pre-3.3.0 JavaScript SDKs.",
    "1-1": "The maximum duration in milliseconds that an event can exist in the queue before being flushed.\nDefault: 30000 ms",
    "1-0": "**eventFlushInterval** ",
    "h-2": "Recommended Value",
    "h-3": "Server",
    "0-2": "Based on your organization's requirements.",
    "1-2": "Based on your organization's requirements.",
    "0-3": "",
    "1-3": "Based on your organization's requirements."
  },
  "cols": 3,
  "rows": 2
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The maximum payload size is 10 MB. If the resulting batch payload exceeds this limit, requests will be rejected with a 400 response code, `Bad Request Error`.\n\nThe most common cause of a large payload size is a high batch size. If your payloads are exceeding the size limit, try configuring a smaller batch size."
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
    "0-0": "**close()** ",
    "h-0": "Method",
    "h-1": "Description",
    "0-1": "Stops all timers and flushes the event queue. This method will also stop any timers that are happening for the datafile manager. \n\n**Note**: We recommend that you connect this method to a kill signal for the running process."
  },
  "cols": 2,
  "rows": 1
}
[/block]
You can hook into the `SIGTERM` event to ensure that `close()` is invoked, which guarantees that all events in the queue are sent.
[block:code]
{
  "codes": [
    {
      "code": "// respond to SIGTERM and close optimizely\nprocess.on('SIGTERM', async () => {\n  console.log('SIGTERM signal received.');\n  await optimizely.close()\n  process.exit(0)\n});\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]