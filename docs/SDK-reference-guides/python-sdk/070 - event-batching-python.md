---
title: "Event batching"
slug: "event-batching-python"
hidden: true
createdAt: "2020-04-28T03:44:25.926Z"
updatedAt: "2020-04-28T15:01:32.837Z"
---
The Optimizely Full Stack Python SDK lets you batch events and includes options to set a maximum batch size and flush interval timeout. The benefit of event batching means less network traffic for the same number of impression and conversion events tracked. 

The event batching functionality works by processing impression events from [Activate](doc:activate-python), [Is Feature Enabled](doc:is-feature-enabled-python) and conversion events from [Track](doc:track-python) and placing them into a queue. The queue is drained when it either reaches its maximum size limit or when the flush interval is triggered.

By default, event batching is disabled in Python SDK 3.3.0. Follow along to learn how to take advantage of event batching in the Python SDK.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Event batching works with both out-of-the-box and custom event dispatchers.\n\nMake sure that you aren't sending personally identifiable information (PII) to Optimizely. The event batching process doesn't remove PII from events."
}
[/block]

[block:api-header]
{
  "title": "Configure event batching"
}
[/block]
Event batching can be enabled through the usage of `BatchEventProcessor`. We provide two main options to configure event batching: `batch_size` and `flush_interval`. You can pass in both these options when creating instance of `BatchEventProcessor` and pass the created instance during Optimizely client creation. When using `BatchEventProcessor`, events are held in a queue until either:
* The number of events reaches the defined `batch_size`.
* The oldest event has been in the queue for longer than the defined `flush_interval`, which is specified in seconds. The queue is then flushed and all queued events are sent to Optimizely in a single network request.
* A new datafile revision is received. This occurs only when live datafile updates are enabled. See [Enable automatic datafile updates](doc:update-datafiles-automatically).
[block:code]
{
  "codes": [
    {
      "code": "from optimizely import optimizely\nfrom optimizely import event_dispatcher as optimizely_event_dispatcher\nfrom optimizely.event import event_processor\n\n# Set event dispatcher that the \n# You can reference your own implementation of event dispatcher here\nevent_dispatcher = optimizely_event_dispatcher.EventDispatcher\n\n# Create instance of BatchEventProcessor.\n#\n# In this example here we set batch size to 15 events\n# and flush interval to 50 seconds. \n# Setting start_on_init starts the consumer\n# thread to start receiving events.\n#\n# See table below for explanation of these\n# and other configuration options.  \nbatch_processor = event_processor.BatchEventProcessor(\n    event_dispatcher,\n    batch_size=15,\n    flush_interval=50\n    start_on_init=True,\n)\n\n# Create Optimizely client and pass in instance \n# of BatchEventProcessor to enable batching.\noptimizely_client = optimizely.Optimizely(\n    sdk_key='<Your SDK Key here>', \n    datafile='<Your datafile here>',\n    event_processor=batch_processor\n)\n",
      "language": "python"
    }
  ]
}
[/block]
The table below defines these and other options that you can use to configure the BatchEventProcessor. 
[block:parameters]
{
  "data": {
    "h-0": "Name",
    "h-1": "Default value",
    "0-0": "**event_dispatcher** ",
    "0-1": "No default value. Required parameter.",
    "1-1": "NoOpLogger",
    "1-0": "**logger** \n*optional*",
    "h-2": "Description",
    "h-3": "Server",
    "0-2": "An event handler to manage network calls. \n\nProvides a `dispatch_event` method that takes in URL and parameters and dispatches request.",
    "1-2": "A logger implementation to log issues.",
    "0-3": "Based on your organization's requirements.",
    "1-3": "Based on your organization's requirements.",
    "2-0": "**flush_interval**\n*optional*",
    "2-1": "30",
    "3-0": "**batch_size**\n*optional*",
    "3-1": "10",
    "3-2": "The maximum number of events to hold in the queue. Once this number is reached, all queued events are flushed and sent to Optimizely.",
    "2-2": "The maximum duration in seconds that an event can exist in the queue before being flushed.",
    "4-0": "**start_on_init** \n*optional*",
    "4-1": "False",
    "4-2": "Boolean which if set true starts the thread consuming and queuing events on initializing the `BatchEventProcessor`.\n\nBy default the value is False and so the consumer thread is not ready to receive any events. One can always start the consumer thread by calling `start` on the instance of `BatchEventProcessor`",
    "5-0": "**timeout_interval** \n*optional*",
    "7-0": "**notification_center** \n*optional*",
    "6-0": "**event_queue** \n*optional*",
    "7-2": "Instance of `optimizely.notification_center.NotificationCenter`.\n\nBy default an instance of `notification_center.NotificationCenter` is created.",
    "7-1": "NotificationCenter",
    "6-1": "six.moves.queue.Queue",
    "6-2": "Component that allows you to accumulate events until they are dispatched.",
    "5-1": "5",
    "5-2": "Number representing time interval in seconds before joining the consumer thread."
  },
  "cols": 3,
  "rows": 8
}
[/block]
For more information, see [Initialize SDK](doc:initialize-sdk-python).
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "The maximum payload size is 10 MB. If the resulting batch payload exceeds this limit, requests will be rejected with a 400 response code, `Bad Request Error`."
}
[/block]

[block:api-header]
{
  "title": "Side effects"
}
[/block]
The table lists other Optimizely functionality that may be triggered by using this class.
[block:parameters]
{
  "data": {
    "0-0": "LogEvent",
    "0-1": "Whenever the event processor produces a batch of events, a LogEvent object will be created using the EventFactory.\nIt contains batch of conversion and impression events. \nThis object will be dispatched using the provided event dispatcher and also it will be sent to the notification subscribers.",
    "1-0": "Notification Listeners",
    "1-1": "Flush invokes the LOG_EVENT [notification listener](doc:set-up-notification-listener-python) if this listener is subscribed to.",
    "h-0": "Functionality",
    "h-1": "Description"
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
      "code": "def on_log_event():\n  pass\n\noptimizely_client.notification_center.add_notification_listener(enums.NotificationTypes.LOG_EVENT, on_log_event)",
      "language": "python"
    }
  ]
}
[/block]
###  LogEvent

LogEvent object gets created using [EventFactory](https://github.com/optimizely/python-sdk/blob/master/optimizely/event/event_factory.py). It represents the batch of impression and conversion events we send to the Optimizely backend.
[block:parameters]
{
  "data": {
    "0-0": "**http_verb**\n*Required (non null)*",
    "h-0": "Object",
    "h-1": "Type",
    "h-2": "Description",
    "0-2": "The HTTP verb to use when dispatching the log event. It can be Get or Post.",
    "1-0": "**url**\n*Required (non null)*",
    "2-0": "**params**\n*Required (non null)* ",
    "3-0": "**headers**",
    "1-2": "URL to dispatch log event to.",
    "2-2": "Event Batch. It contains all the information regarding every event which is batched. including list of visitors which contains UserEvent.",
    "3-2": "Request Headers",
    "3-1": "Dict",
    "1-1": "String",
    "0-1": "String",
    "2-1": "Dict"
  },
  "cols": 3,
  "rows": 4
}
[/block]

[block:api-header]
{
  "title": "Stop BatchEventProcessor on application exit"
}
[/block]
If you enable event batching, it's important that you call the 'stop' method, `batch_processor.stop()`, prior to exiting. This ensures that queued events are flushed as soon as possible to avoid any data loss.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "Because the BatchEventProcessor maintains a buffer of queued events, we recommend that you call `stop()` on the BatchEventProcessor instance before shutting down your application."
}
[/block]

[block:parameters]
{
  "data": {
    "0-0": "**stop()**",
    "h-0": "Method",
    "h-1": "Description",
    "0-1": "Stops and flushes the event queue. \n\n**Note**: We recommend that you connect this method to a kill signal for the running process."
  },
  "cols": 2,
  "rows": 1
}
[/block]