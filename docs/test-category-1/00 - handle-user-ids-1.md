---
title: "Handle user IDs"
slug: "handle-user-ids"
hidden: false
createdAt: "2019-08-21T19:32:15.818Z"
updatedAt: "2019-09-27T21:56:04.762Z"
---
*User IDs* uniquely identify the users who are included when you roll out a feature. Supply any string you want for user IDs.

For example, if you're rolling out features to anonymous users, you can use a first-party cookie or device ID to identify each participant. If you're rolling out features to known users, you can use a universal user identifier (UUID) to identify each participant. If you're using UUIDs, you can roll out features that span multiple devices or applications and ensure that users have a consistent treatment.

User IDs don't necessarily need to correspond to individual users. If you're rolling out features in a business application, you may want to pass account IDs to the SDK to ensure that every user in a given account has the same treatment.

Here are more tips for using user IDs:

* *Ensure user IDs are unique*: User IDs must be unique among the population included in your rollout. Optimizely buckets users based on the user IDs that you provide.

* *Use IDs from third-party platforms*: If you are measuring the impact of your feature in a third-party analytics platform, we recommend leveraging the same user ID from that platform. This helps ensure data consistency and make it easier to reconcile events between systems.

* *Use either logged-out or logged-in IDs*: Optimizely doesn't currently provide a mechanism to alias logged-out IDs with logged-in IDs. If you are rolling out features that span both logged-out and logged-in states, you must persist logged-out IDs for the lifetime of the feature.