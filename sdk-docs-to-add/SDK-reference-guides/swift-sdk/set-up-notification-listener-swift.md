---
title: "Set up notification listener"
slug: "set-up-notification-listener-swift"
hidden: false
createdAt: "2019-09-12T16:59:47.582Z"
updatedAt: "2020-02-10T20:01:33.586Z"
---
Notification listeners trigger a callback function that you define when certain actions are triggered in the SDK. 

The most common use case is to send a stream of all feature flag decisions to an analytics provider or to an internal data warehouse to join it with other data that you have about your users.

To track feature usage:
1. Sign up for an analytics provider of your choice (for example, Segment)
2. Set up a notification listener of type 'DECISION'
3. Follow your analytics provider's documentation and send events from within the decision listener callback

Steps 1 and 3 aren't covered in this documentation. However, setting up a 'DECISION' notification listener is covered below.
[block:api-header]
{
  "title": "Set up a DECISION notification listener"
}
[/block]
The `DECISION` notification listener enables you to be notified whenever the SDK determines what decision value to return for a feature. The callback is triggered with the decision type, associated decision information, user ID, and attributes.

`DECISION` listeners are triggered in multiple cases. Please see the tables at the end of this section for complete detail.

To set up a `DECISION` listener:
  1. Define a callback to be called when the `DECISION` event is triggered
  2. Add the callback to the notification center on the Optimizely instance

The example code below shows how to add a listener, remove a listener, remove all listeners of a specific type (such as all decision listeners), and remove all listeners.
[block:code]
{
  "codes": [
    {
      "code": "// Add a notification listener\nlet notificationId = optimizely.notificationCenter.addDecisionNotificationListener(decisionListener: { (type, userId, attributes, decisionInfo) in    \n    // Send data to analytics provider here\n})  \n\n// Remove a specific notification listener\noptimizely.notificationCenter.removeNotificationListener(notificationId: notificationId!)\n\n// Remove notification listeners of a certain type\noptimizely.notificationCenter.clearNotificationListeners(type: .decision)\n\n// Remove all notification listeners\noptimizely.notificationCenter.clearAllNotificationListeners()  \n\n",
      "language": "swift"
    },
    {
      "code": "// Add a notification listener\nNSNumber *notificationId = [self.optimizely.notificationCenter addDecisionNotificationListenerWithDecisionListener:^(NSString *type, NSString *userId, NSDictionary<NSString *,id> *attributes, NSDictionary<NSString *,id> *decisionInfo) {\n    // Send data to analytics provider here\n}];\n\n// Remove a specific listener\n[optimizely.notificationCenter removeNotificationListenerWithNotificationId:notificationId];\n\n// Remove all of one type of listener\n[optimizely.notificationCenter clearNotificationListenersWithType:NotificationTypeDecision];\n\n// Remove all notification listeners\n[optimizely.notificationCenter clearAllNotificationListeners];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
The tables below show the information provided to the listener when it is triggered.
[block:parameters]
{
  "data": {
    "0-0": "**type**",
    "0-1": "string",
    "0-2": "- `feature`: Returned when you use the Is Feature Enabled to determine if user has access to one specific feature, or Get Enabled Features method to determine if user has access to multiple features.\n\n- `ab-test`: Returned when you use activate or get_variation to determine the variation for a user, and the given experiment is not associated to any feature.\n\n- `feature-test`: Returned when you use activate or get_variation to determine the variation for a user, and the given experiment is associated to some feature.\n\n- `feature-variable`: Returned when you use one of the get_feature_variable methods to determine value of some feature variable. Such as get_feature_variable_boolean.",
    "1-0": "**decision info**",
    "1-1": "map",
    "2-0": "**user ID**",
    "2-1": "string",
    "3-0": "**attributes**",
    "3-1": "map",
    "1-2": "Key-value map that consists of data corresponding to the decision and based on the `type`.\nSee the table below for valid fields and values for each `type`.",
    "h-0": "Field",
    "h-1": "Type",
    "h-2": "Description",
    "2-2": "The user ID.",
    "3-2": "A map of custom key-value string pairs specifying attributes for the user that are used for audience targeting. Non-string values are only supported in the 3.0 SDK and above."
  },
  "cols": 3,
  "rows": 4
}
[/block]

[block:parameters]
{
  "data": {
    "0-1": "- `featureKey`: String id of the feature.\n- `featureEnabled`: True or false based on whether the feature is enabled for the user.\n- `source`: String denoting how user gained access to the feature. Value is:\n -  `feature-test` if the feature became enabled or disabled for the user because of some experiment associated with the feature.\n -  `rollout` if the feature became enabled or disabled for the user because of the rollout configuration associated with the feature.\n- `sourceInfo`: Empty if the source is rollout. Holds experimentKey and variationKey if the source is feature-test.",
    "0-0": "**feature**",
    "h-0": "Type",
    "h-1": "Decision Info Values",
    "1-0": "**ab-test**",
    "2-0": "**feature-test**",
    "3-0": "**feature-variable**",
    "1-1": "- `experimentKey`: String key of the experiment\n- `variationKey`: String key of the variation to which the user got bucketed.",
    "2-1": "- `experimentKey`: String key of the experiment\n- `variationKey`: String key of the variation to which the user got bucketed.",
    "3-1": "- `featureKey`: String id of the feature.\n- `featureEnabled`: True or false based on whether the feature is enabled for the user.\n- `source`: String denoting how user gained access to the feature. Value is:\n -  `feature-test` if the feature became enabled or disabled for the user because of some experiment associated with the feature.\n -  `rollout` if the feature became enabled or disabled for the user because of the rollout configuration associated with the feature.\n- `variableKey`: String key of the feature variable.\n- `variableValue`: Mixed value of the feature variable for this user.\n- `variableType`: String type of the feature variable. Can be one of boolean, double, integer, string.\n- `sourceInfo`: Map denoting source of decision. Empty if the source is rollout. Holds experimentKey and variationKey if the source is feature-test."
  },
  "cols": 2,
  "rows": 4
}
[/block]

[block:api-header]
{
  "title": "Access test and variation identifiers"
}
[/block]
Keys and IDs can be accessed on `OPTLYExperiment` objects using `experimentKey` and `experimentId` respectively. Likewise, keys and IDs can be accessed on `OPTLYVariation` objects using `variationKey` and `variationId`.
[block:callout]
{
  "type": "info",
  "body": "Experiment keys are unique within an Optimizely project, and variation keys are unique within an experiment. However, experiment keys and variation keys are not guaranteed to be *universally *unique because they are user generated. This may become a problem in your analytics reports if you use the same key in multiple Optimizely projects or rename your keys.\n\nFor human-friendly strings when absolute uniqueness is not required, use keys. If you need to uniquely identify experiments and variations in a way that will not change when you rename keys, use the automatically generated IDs.",
  "title": "Note"
}
[/block]