---
title: "Event batching"
slug: "event-batching-react"
hidden: true
createdAt: "2019-09-26T17:12:59.488Z"
updatedAt: "2019-09-26T18:10:24.183Z"
---
The React SDK lets you batch events and includes options to set a maximum batch size and flush interval timeout. The benefit of event batching means less network traffic for the same number of Impression and conversion events tracked. 

The event batching functionality works by processing impression events from [OptimizelyExperiment](doc:optimizelyexperiment) or [OptimizelyFeature](doc:optimizelyfeature), and conversion events from calling track on the client instance, placing them into a queue. The queue is drained when it either reaches its maximum size limit or when the flush interval is triggered.

By default, event batching is enabled in React SDK.
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
      "code": "import { createInstance } from '@optimizely/react-sdk';\n\nconst optimizely = createInstance({\n  datafile: window.datafile // assuming you have a datafile at window.datafile\n  // other options\n  eventBatchSize: 100,\n  eventFlushInterval: 3000,\n});\n\n",
      "language": "javascript",
      "name": "JavaScript"
    },
    {
      "code": "const optimizelySdk = require('@optimizely/optimizely-sdk')\n\noptimizelySdk.createInstance({\n  // other options\n  eventBatchSize: 100,\n  eventFlushInterval: 3000,\n})\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]
The table below defines these options and lists general recommendations for browser and server default settings. On the browser we recommend using a small `eventBatchSize` (10) and a short `eventFlushInterval` (1000). This ensures that events are sent in a relatively fast manner, since some events could be lost if a user immediately bounces, while gaining network efficiency in cases where many events are generated back-to-back.

For the server, these numbers are more flexible and should be set based on your organization's traffic and needs. Please [contact support](https://help.optimizely.com/Account_Settings/File_online_tickets_for_support) for recommendations for your specific implementation.
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Description",
    "0-0": "**eventBatchSize** ",
    "0-1": "The maximum number of events to hold in the queue. Once this number is reached, all queued events are flushed and sent to Optimizely.\nDefault: 10\n\n**Note**: By setting this value to 1, events aren't batched and event requests behave exactly as in pre-3.3.0 JavaScript and Node SDKs.",
    "1-1": "The maximum duration in milliseconds that an event can exist in the queue before being flushed.\nDefault: 1000 ms in the browser, 30000 ms in Node.js",
    "1-0": "**eventFlushInterval** ",
    "h-2": "Browser",
    "h-3": "Server",
    "0-2": "10",
    "1-2": "1000",
    "0-3": "Based on your organization's requirements.",
    "1-3": "Based on your organization's requirements."
  },
  "cols": 4,
  "rows": 2
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The maximum payload size is 10 MB. If the resulting batch payload exceeds this limit, requests will be rejected with a 400 response code, `Bad Request Error`."
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
On the browser side, `optimizely.close()` is automatically connected to the [pagehide event](https://developer.mozilla.org/en-US/docs/Web/API/Window/pagehide_event), so there is no need to do any manual instrumentation.