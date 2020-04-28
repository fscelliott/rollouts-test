---
title: "Integrate with analytics platforms"
slug: "configure-analytics"
hidden: false
createdAt: "2019-08-21T19:32:15.839Z"
updatedAt: "2019-09-27T21:56:04.764Z"
---
When rolling out features, you'll want to know when users are accessing a feature flag and what experience they're receiving in production.

Although Optimizely Rollouts doesn't offer analytics on feature flags out of the box, you can still track analytics on feature flag usage by adding a notification listener to send events to the analytics provider of your choice.

Notification listeners trigger a callback function that you define when certain actions are triggered in the SDK. 

The most common use case is to send a stream of all feature flag decisions to an analytics provider or to an internal data warehouse to join it with other data that you have about your users.

To track feature usage:
1. Sign up for an analytics provider of your choice (for example, Segment)
2. Set up a notification listener of type 'DECISION'
3. Follow your analytics provider's documentation and send events from within the decision listener callback.

In our SDK reference guides, see how to set up a DECISION notification listener.