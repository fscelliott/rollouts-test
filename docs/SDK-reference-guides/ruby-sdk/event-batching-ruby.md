---
title: "Event batching"
slug: "event-batching-ruby"
hidden: false
createdAt: "2019-09-12T16:41:25.127Z"
updatedAt: "2019-12-13T00:40:20.588Z"
---
The Optimizely Ruby SDK batches impression and conversion events into a single payload before sending it to Optimizely. This is achieved through an SDK component called the event processor.

Event batching has the advantage of reducing the number of outbound requests to Optimizely depending on how you define, configure, and use the event processor. It means less network traffic for the same number of impression and conversion events tracked.

In the Ruby SDK, `BatchEventProcessor` provides implementation of the `EventProcessor` interface and batches events. You can control batching based on two parameters:
* Batch size: Defines the number of events that are batched together before sending to Optimizely.
* Flush interval: Defines the amount of time after which any batched events should be sent to Optimizely.

An event that consists of the batched payload is sent as soon as the batch size reaches the specified limit or flush interval reaches the specified time limit. `BatchEventProcessor` options are described in more detail below.
[block:api-header]
{
  "title": "Basic example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "require 'optimizely'\nrequire 'optimizely/optimizely_factory'\n\n# Initialize an Optimizely client\noptimizely_instance = Optimizely::OptimizelyFactory.default_instance(\n  'put_your_sdk_key_here'\n)\n\n",
      "language": "ruby"
    }
  ]
}
[/block]
By default, batch size is 10 and flush interval is 30 seconds.
[block:api-header]
{
  "title": "Advanced example"
}
[/block]
Set the batch size and flush interval using BatchEventProcessor's constructor.
[block:code]
{
  "codes": [
    {
      "code": "require 'optimizely'\nrequire 'optimizely/event/batch_event_processor'\n\n# Initialize BatchEventProcessor\nevent_processor = Optimizely::BatchEventProcessor.new(\n  event_dispatcher: event_dispatcher,\n  batch_size: 50,\n  flush_interval: 1000\n)\n\n# Initialize an Optimizely client\noptimizely_client = Optimizely::Project.new(\n  datafile,\n  event_dispatcher,\n  logger,\n  error_handler,\n  skip_json_validation,\n  user_profile_service,\n  sdk_key,\n  config_manager,\n  notification_center,\n  event_processor\n)\n\n",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "BatchEventProcessor"
}
[/block]
`BatchEventProcessor` is an implementation of `EventProcessor` where events are batched. The class maintains a single consumer thread that pulls events off of the `queue` and buffers them for either a configured batch size or a maximum duration before the resulting `LogEvent` is sent to the `EventDispatcher` and `NotificationCenter`.

The following properties can be used to customize the `BatchEventProcessor` configuration.
[block:parameters]
{
  "data": {
    "h-0": "Property",
    "h-1": "Default value",
    "h-2": "Description",
    "0-0": "**event_queue**",
    "0-1": "1000",
    "0-2": "SizedQueue.new(100) or Queue.new\nQueues individual events to be batched and dispatched by the executor.\nDefault value is 1000.",
    "2-0": "**batch_size**",
    "2-1": "10",
    "2-2": "The maximum number of events to batch before dispatching. Once this number is reached, all queued events are flushed and sent to Optimizely.",
    "3-0": "**flush_interval**",
    "3-1": "30000",
    "3-2": "Maximum time to wait before batching and dispatching events. In milliseconds.",
    "4-0": "**notification_center**",
    "4-1": "nil",
    "4-2": "Notification center instance to be used to trigger any notifications.",
    "1-0": "**event_dispatcher**",
    "1-1": "nil",
    "1-2": "Used to dispatch event payload to Optimizely."
  },
  "cols": 3,
  "rows": 5
}
[/block]
For more information, see [Initialize SDK](doc:initialize-sdk-ruby).
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
    "0-0": "LogEvent",
    "0-1": "Whenever the event processor produces a batch of events, a LogEvent object will be created using EventFactory.\nIt contains batch of conversion and impression events. \nThis object will be dispatched using the provided event dispatcher and also it will be sent to the notification subscribers",
    "1-0": "Notification Listeners",
    "1-1": "Flush invokes the LOG_EVENT [notification listener](doc:set-up-notification-listener-ruby) if this listener is subscribed to."
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
      "code": "callback_reference = lambda do |*args|\n        puts \"Notified!\"\nend\n\noptimizely_client.notification_center.add_notification_listener(        Optimizely::NotificationCenter::NOTIFICATION_TYPES[:LOG_EVENT], callback_reference )\n\n",
      "language": "ruby"
    }
  ]
}
[/block]
###  LogEvent

LogEvent object gets created using [EventFactory](https://github.com/optimizely/ruby-sdk/blob/master/lib/optimizely/event/event_factory.rb). It represents the batch of impression and conversion events we send to the Optimizely backend.
[block:parameters]
{
  "data": {
    "h-0": "Object",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**http_verb**\n*Required (non null)*",
    "0-1": "String",
    "0-2": "The HTTP verb to use when dispatching the log event. It can be Get or Post.",
    "1-0": "**url**\n*Required (non null)*",
    "1-1": "String",
    "1-2": "URL to dispatch log event to.",
    "2-0": "**params**\n*Required (non null)*",
    "2-1": "EventBatch",
    "2-2": "It contains all the information regarding every event which is batched. including list of visitors which contains UserEvent.",
    "3-0": "**headers**\n*Required*",
    "3-1": "Hash",
    "3-2": "Request Headers."
  },
  "cols": 3,
  "rows": 4
}
[/block]

[block:api-header]
{
  "title": "Close Optimizely on application exit"
}
[/block]
If you enable event batching, make sure that you call the `close` method, `optimizely.close()`, prior to exiting. This ensures that queued events are flushed as soon as possible to avoid any data loss.
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