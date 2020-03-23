---
title: "Set up notification listener"
slug: "set-up-notification-listener"
hidden: false
createdAt: "2019-09-27T21:57:10.424Z"
updatedAt: "2019-09-27T22:01:03.204Z"
---
Notification listeners trigger a callback function that you define when certain actions are triggered in the SDK. 

The most common use case is to send a stream of all feature flag decisions to an analytics provider or to an internal data warehouse to join it with other data that you have about your users.

To track feature usage:
1. Sign up for an analytics provider of your choice (for example: Segment)
2. Set up a notification listener of type 'DECISION'
3. Follow your analytics provider's documentation and send events from within the decision listener callback

Steps 1 and 3 aren't covered in this documentation. However, setting up a 'DECISION' notification listener is covered below.
[block:api-header]
{
  "title": "Set up a DECISION notification listener"
}
[/block]
The `DECISION` notification listener enables you to be notified whenever the SDK determines what decision value to return for a feature. The callback is triggered with the decision type, associated decision information, user ID, and attributes.

`DECISION` listeners are triggered when your application renders [OptimizelyFeature](doc:optimizelyfeature) or [OptimizelyExperiment](doc:optimizelyexperiment) or when you call the [Is Feature Enabled](doc:is-feature-enabled-react) or [Get Enabled Features](doc:get-enabled-features-react) APIs.

To set up a `DECISION` listener:
  1. Define a callback to be called when the `DECISION` event is triggered
  2. Add the callback to the notification center on the Optimizely instance

The example code below shows how to add a listener, remove a listener, remove all listeners of a specific type (such as all decision listeners), and remove all listeners.
[block:code]
{
  "codes": [
    {
      "code": "import {\n  createInstance,\n  enums,\n} from '@optimizely/react-sdk';\n\nconst optimizely = createInstance({\n  sdkKey: 'CxpSJttFVpz8LiLg8jnZwq',\n})\n\nfunction onDecision(decisionObject) {\n  if (decisionObject.type === 'feature') {\n    // Access type on decisionObject to get type of decision\n    console.log(decisionObject.type);\n    // Access decisionInfo on decisionObject which\n    // will have form as per type of decision.\n    console.log(decisionObject.decisionInfo);\n\n    // Send data to analytics provider here\n    // Example:\n    //  analytics.track({\n    //    userId: decisionObject.userId,\n    //    event: decisionObject.type,\n    //    properties: decisionObject.decisionInfo,\n    //  });\n  }\n}\n\n// Add a notificaiton listener for the decision notification\nconst listenerId = optimizely.notificationCenter.addNotificationListener(\n  enums.NOTIFICATION_TYPES.DECISION,\n  onDecision,\n);\n\n// Remove a specific listener\noptimizely.notificationCenter.removeNotificationListener(listenerId);\n\n// Remove listeners for a specific notification type\noptimizely.notificationCenter.clearNotificationListeners(\n  enums.NOTIFICATION_TYPES.DECISION,\n);\n\n// Remove all listeners\noptimizely.notificationCenter.clearAllNotificationListeners();\n",
      "language": "javascript"
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
    "0-2": "- `feature`: Returned when you use the Is Feature Enabled to determine if user has access to one specific feature, or Get Enabled Features method to determine if user has access to multiple features.\n - Is Feature Enabled returns a boolean value.\n - Get Enabled Features returns an array of strings consisting of all features in the project enabled for the user.",
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
    "0-1": "- `feature_key`: String id of the feature.\n- `feature_enabled`: True or false based on whether the feature is enabled for the user.\n- `source`: String denoting how user gained access to the feature. Value is:\n -  `rollout` if the feature became enabled or disabled for the user because of the rollout configuration associated with the feature.",
    "0-0": "**feature**",
    "h-0": "Type",
    "h-1": "Decision Info Values"
  },
  "cols": 2,
  "rows": 1
}
[/block]