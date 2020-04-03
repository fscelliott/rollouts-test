---
title: "Interactions between feature tests and rollouts"
slug: "interactions-between-feature-tests-and-rollouts"
hidden: false
createdAt: "2019-09-11T11:47:39.205Z"
updatedAt: "2019-09-12T18:48:34.946Z"
---
**Feature tests take precedence over feature rollouts**. This means that if your feature has both a feature test and rollout, the feature test is evaluated first. If a user does not qualify for the feature test, the rollout is evaluated.

The table lists the differences between feature tests and rollouts.
[block:parameters]
{
  "data": {
    "0-0": "Purpose",
    "1-0": "User impressions",
    "0-1": "Lets you gather metrics data so you can compare multiple variations.",
    "0-2": "Lets you deploy a feature that you've already tested or measured some other way, such as qualitative feedback.",
    "1-1": "When a user is assigned to a feature test, Optimizely sends an impression so that information is recorded in your test results.",
    "1-2": "When a user is assigned to a rollout, Optimizely does not send an impression. This means that extra network traffic and test results aren't generated. \n\n**Note: Rollouts don't count against your impressions limit**.",
    "h-1": "Feature test",
    "h-2": "Feature rollout",
    "2-0": "Variations",
    "2-1": "Test multiple variations—on versus off, or a multivariate test of different configurations",
    "2-2": "Only toggles the feature on or off."
  },
  "cols": 3,
  "rows": 3
}
[/block]

[block:api-header]
{
  "title": "Example scenario"
}
[/block]
To understand how this works in practice, imagine the following scenario. You've deployed a feature to your application and you've created both a feature test and rollout for that feature:

 - Feature test running with an audience and a traffic allocation
 - Rollout running with an audience and a traffic allocation

A series of users are handled by your application and are evaluated according to their audience attributes and your feature test and feature rollout rules. The table depicts all possible outcomes.
[block:parameters]
{
  "data": {
    "h-0": "User",
    "h-1": "Feature Test Audience",
    "h-2": "Feature Test Traffic Allocation",
    "h-3": "Feature Rollout Audience",
    "h-4": "Feature Rollout Traffic Allocation",
    "h-5": "Result",
    "0-0": "user1",
    "1-0": "user2",
    "2-0": "user3",
    "3-0": "user4",
    "4-0": "user5",
    "0-1": "pass",
    "1-1": "pass",
    "2-1": "fail",
    "3-1": "fail",
    "4-1": "fail",
    "0-2": "pass",
    "1-2": "fail",
    "2-2": "N/A",
    "3-2": "N/A",
    "4-2": "N/A",
    "4-4": "N/A",
    "0-4": "N/A",
    "0-3": "N/A",
    "1-3": "pass",
    "2-3": "pass",
    "3-3": "pass",
    "4-3": "fail",
    "1-4": "pass",
    "2-4": "pass",
    "3-4": "fail",
    "0-5": "Feature test",
    "1-5": "Feature rollout",
    "2-5": "Feature rollout",
    "3-5": "No action",
    "4-5": "No action"
  },
  "cols": 6,
  "rows": 5
}
[/block]

[block:api-header]
{
  "title": "Is Feature Enabled method"
}
[/block]
Feature tests and rollouts are both triggered using the Is Feature Enabled method. 
[block:api-header]
{
  "title": "Interactions in multiple feature tests"
}
[/block]
In each environment, you can only run one feature test on each feature at a time (meaning, you can't run two tests on the same feature simultaneously) unless your feature is assigned to a [mutually exclusive group](doc:use-mutual-exclusion). If you try to launch a second feature test while another feature test is running, you’ll see a warning that only one feature test is allowed at a time&mdash;unless the tests are mutually exclusive.

[Use mutually exclusive groups](doc:use-mutual-exclusion) to run concurrent production feature tests. If feature tests are running in a mutually exclusive group, Optimizely evaluates a user’s mutually exclusive group first in the order of operations after calling the Is Feature Enabled method. After the mutually exclusive group is assigned, Optimizely evaluates the feature test first. If a user does not quality for the feature test, the feature rollout is evaluated.

Note that you can have as many draft, paused, and archived tests on a feature as you like.