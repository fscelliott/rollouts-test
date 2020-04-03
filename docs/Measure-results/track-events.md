---
title: "Track events"
slug: "track-events"
hidden: false
createdAt: "2019-09-11T11:47:39.193Z"
updatedAt: "2020-02-21T19:07:36.355Z"
---
You can track conversion events from your code with the Track method. This method requires:
 - **event key**: The event key should match the event key you provided when [creating events](doc:create-events) in the Optimizely app.
 - **user ID**: The user ID must match the ID provided in the Activate or Is Feature Enabled methods.

Track also takes optional user **attributes**. Optimizely uses attributes passed to Track for [segmentation on the Results page](analyze-results#section-segment-results). You must pass the user attributes required to meet the audience condition(s) for the experiment(s) in order for the event to be tracked.

As an example, think of an experiment that targets based on location and also passes in device information. That device information could change from event to event, but the location attribute needs to be in there in order to meet the audience condition.

The example below shows Track called with user attributes.
[block:code]
{
  "codes": [
    {
      "code": "// Track a conversion event for the provided user with attributes\noptimizelyClient.track(eventKey, userId, attributes);\n\n",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "// Track a conversion event for the provided user with attributes\nOptimizelyClient.Track(eventKey, userId, attributes);\n\n",
      "language": "csharp"
    },
    {
      "code": "// Track a conversion event for the provided user with attributes\noptimizelyClient.track(eventKey, userId, attributes);\n\n",
      "language": "java"
    },
    {
      "code": "// Track a conversion event for the provided user with attributes\noptimizelyClient.track(eventKey, userId, attributes);\n\n",
      "language": "javascript",
      "name": "JavaScript"
    },
    {
      "code": "// Track a conversion event for the provided user with attributes\noptimizelyClient.track(\"my_conversion\", userId, {\n  plan_type: \"silver\"\n});\n\n",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "// Track a conversion event for the provided user with attributes\n[optimizely track:@\"my_conversion\"\n           userId:@\"user123\"\n       attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "// Track a conversion event for the provided user with attributes\n$optimizelyClient->track($eventKey, $userId, $attributes);\n\n",
      "language": "php"
    },
    {
      "code": "# Track a conversion event for the provided user with attributes\noptimizely_client.track(event_key, user_id, attributes)\n\n",
      "language": "python"
    },
    {
      "code": "# Track a conversion event for the provided user with attributes\noptimizely_client.track(event_key, user_id, attributes)\n\n",
      "language": "ruby"
    },
    {
      "code": "// Track a conversion event for the provided user with attributes\noptimizely?.track(\"my_conversion\", userId:\"user123\", attributes:attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
For more information about when and how to track events, see the other topics in this section.
[block:api-header]
{
  "title": "Usage notes"
}
[/block]
The Track function can be used to track events across multiple experiments. It will be counted for each experiment only if Activate or Is Feature Enabled has previously been called for the current user.

As long as the event key is valid (meaning the key appears in the datafile), Optimizely will track events. We recommend that you use attributes when you track events for segmentation purposes.
[block:callout]
{
  "type": "warning",
  "body": "**The [Results page](doc:analyze-results) only shows events that are tracked after Activate or Is Feature Enabled has been called**. If you do not see results on the Results page, make sure that you are activating the experiment or evaluating the feature flag before tracking conversion events.",
  "title": "Important"
}
[/block]

[block:api-header]
{
  "title": "Track events across platforms"
}
[/block]
For offline event tracking and other advanced use cases, you can also use the [Events API](https://developers.optimizely.com/x/events/api/).

You can use any of our SDKs to track events, so you can run experiments that span multiple applications, services, or devices. All of our SDKs have the same audience evaluation and targeting behavior, so you'll see the same output from experiment activation and tracking as long as you are using the same datafile and user IDs.

For example, if you're running experiments on your server, you can activate experiments with our Python, Java, Ruby, C#, Node, or PHP SDKs, but track user actions client-side using our JavaScript, Objective-C or Android SDKs.

If you plan to use multiple SDKs for the same project, make sure that all SDKs share the same datafile and user IDs.

For more information, see [Multiple languages](doc:multiple-languages).
[block:api-header]
{
  "title": "Track numeric metrics"
}
[/block]
It’s possible to track nonbinary metrics such as  time on site, revenue, or total value. See [the section on event tags](https://docs.developers.optimizely.com/full-stack/docs/include-event-tags) to understand how to configure the code needed to track them. 
[block:callout]
{
  "type": "warning",
  "body": "The Easy Event Tracking functionality included in the Full Stack v3.0 release changes the results of the Optimizely [Data Export](https://developers.optimizely.com/x/events/export/index.html). \n\nWhen implementing Full Stack v3.0, you must also upgrade to the **Results Exports** functionality to obtain reliable event attribution data. For more information, see the **Important** note in the [changelog](doc:changelog).",
  "title": "Important"
}
[/block]