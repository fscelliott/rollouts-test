---
title: "Implement impressions"
slug: "implement-impressions"
hidden: false
createdAt: "2019-09-11T11:47:39.179Z"
updatedAt: "2019-09-12T19:09:52.650Z"
---
To measure the effects of an experiment, Optimizely tracks which users were exposed to each variation. The SDK tracks each exposure by sending an **impression** event back to Optimizely's servers, triggering a network request via the event dispatcher. An impression is defined as a visitor being exposed to a test, either an experiment or a feature test.

Impressions are used to generate the visitor counts on the [Results](doc:analyze-results) page, which serve as the denominator for measuring conversion rates. They're also used for billing—when you purchase Full Stack, your pricing plan will include a yearly quota of impressions.
[block:api-header]
{
  "title": "Triggering impressions with the SDK"
}
[/block]
You should fire an impression each time a visitor is exposed to a test via the Activate method for A/B Tests or via the Is Feature Enabled method for Feature Tests.

Examples:
  * A new visitor lands on a page and is exposed to an experiment.
  * A visitor refreshes a page and is exposed to the experiment again.
  * A return visitor is exposed to the experiment again. 

### Impressions are sent
 - Any time the Activate method returns a variation.
 - When the user is bucketed into a [feature test](doc:run-feature-tests) via the Is Feature Enabled or Get Enabled Features method, regardless of whether the feature is enabled or disabled in the feature test variation.

### Impressions are not sent
 - Any time the Activate method returns `null` because the user didn't qualify for any experiment.
 - If the user is only exposed to a feature rollout using Get Enabled Features.
 - If neither a test nor rollout is running.
[block:api-header]
{
  "title": "Get variations without sending an impression"
}
[/block]
In some use cases, you may need to make decisions on the server even though the visitor may not get exposed to the test experience at that point in time. 

For example, in a CDN-fronted application, in a micro-services architecture with multiple decision-points, or in a single-page application (SPA) with server-side rendering. 
[block:callout]
{
  "type": "warning",
  "body": "To avoid excess impression usage, consider firing impressions only at the point where the visitor is exposed to the test, rather than at every point in the application where a variation decision is made.",
  "title": "Important"
}
[/block]
Using Activate in a decision point before your user is exposed to the experience can result in: 
  * Inflated impression counts, where the your end user may never actually see the experience that's being counted.
  * Inflated visitor counts in the experiment and artificially low conversion rates, which may make it more difficult for the experiment to reach statistical significance.

You can use the Get Variation method to capture the variation key without triggering an impression. Get Variation has the exact same behavior as Activate, except that it won't send a network request. This means that if you only use Get Variation, you won't see any results for your experiment.

Use Get Variation when:
- You want to retrieve an experiment decision to pass to another service for later activation.
- You want to perform unit, functional, integration, and other QA tests.
- You need to render static content on the server based on a variation decision, but the visitor may not necessarily be exposed to that experience.

Use Activate when:
- You want to retrieve a variation assignment and also have results to display in Optimizely's reporting interface.
- You need to count a visitor as having been exposed to some variant that was rendered earlier based on a Get Variation call.