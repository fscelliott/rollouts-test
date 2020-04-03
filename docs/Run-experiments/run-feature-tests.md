---
title: "Run feature tests"
slug: "run-feature-tests"
hidden: false
createdAt: "2019-09-11T11:47:39.218Z"
updatedAt: "2019-10-04T23:21:21.287Z"
---
If you set up a new feature that you're building in Optimizely, you can run a feature test on it. A feature test enables you to change the user's experience by creating one variation where the feature is on and another where the feature is off. By tracking performance and engagement metrics in each variation, you can measure the overall impact of the feature on your user experience and your business goals.

You can also test more than two variations, each with different configurations of the [feature variables](doc:define-feature-variables). For example, with a product recommendations feature, you could build variations with different versions of the algorithm, a different label in the interface, or a different number of items. You can test each of these variants without deploying new code. 

See also [Interactions between feature tests and rollouts](doc:interactions-between-feature-tests-and-rollouts). 
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "Don't use the the Activate method with feature tests. If a feature test is running for a given user, Is Feature Enabled will activate it automatically. It will trigger an impression, post to notification listeners, and return the correct values for your feature configuration. Use the Activate method only for a one-off A/B test that isn't impacting a feature that is already implemented."
}
[/block]

[block:api-header]
{
  "title": "Create a feature test"
}
[/block]
You implement feature tests using [features that you've already defined](doc:create-feature-flags). Once you've instrumented a feature flag using the Is Feature Enabled and Get Feature Variable methods, you do not need any additional code. These methods will automatically determine whether the user qualifies for your experiment, which variation they should receive, and the resulting settings for each variable.

To create a new feature test:
1. Navigate to _Experiments > Create New..._.
2. Select _Feature Test_ from the dropdown menu.
3. Choose the feature you want to test or click _Create New Feature..._ to add a new feature.
After you select a feature, Optimizely automatically generates an experiment key by appending “_test” to the end of the feature key for the feature you selected.

You can edit the experiment key if you like, as long as you always use a unique key.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8abb05a-create-feat-test.gif",
        "create-feat-test.gif",
        1428,
        858,
        "#f9fafb"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Create feature test variations"
}
[/block]
Optimizely automatically suggests two customizable variation keys for your feature tests: “variation_1” and “variation_2”. 

When this feature test is live, Optimizely assigns a user to a variation, then returns the values associated with the assigned variation for the Is Feature Enabled method and your feature configuration (meaning, variables). 

To create test variations:
1. Navigate to _Experiments_ and select the appropriate feature test.
2. (Optional) Edit the _Variation Key_ and _Description_ fields in _Variations_.
2. Adjust the _Traffic Distribution_ percentage value, as needed.
3. Toggle the feature _On_ or _Off_.
4. Enter or select the appropriate value for the specific variable key.
5. Click _Save_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/8a31ff6-feature_test_variations.png",
        "feature_test_variations.png",
        696,
        820,
        "#fbfbfc"
      ],
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "body": "If you add variations, Optimizely provides automatic suggestions according to the variation number: “variation_3”, “variation_4”, and so on. Deleting a variation won't affect the automatic numbering of Optimizely's automatic variation key suggestions.",
  "title": "Note"
}
[/block]

[block:api-header]
{
  "title": "Use feature toggles and configurations"
}
[/block]
Feature test variations include a feature toggle and the feature configuration (if one exists). By default, the toggle is set to ON and the configuration default values load.

A common feature test includes a feature with no configuration, with one variation set to test “toggle=ON” and another variation set to test “toggle=OFF.” This enables you to experiment on the performance of your application in its current form versus its performance with your new feature enabled.

If the feature includes a feature configuration and you set a variation to “toggle=OFF,” Optimizely disables the option to modify variable values and reverts to the default variable values.

To create variations using feature configurations, update the variable values under each variation.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f1cadd2-create-feat-test-var.gif",
        "create-feat-test-var.gif",
        1428,
        858,
        "#fcfcfd"
      ],
      "border": true
    }
  ]
}
[/block]
When this feature test is live, the Get Feature Variable method returns the values specified for the variation assigned to a visitor. Experimenting using a feature configuration enables you to iterate on a feature in between code deploys. Run a sequence of experiments with different combinations of variable values to determine the optimal experience for your users.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "If you edit the [feature configuration](doc:configure-feature-variables) for a feature test that is running on a feature configured with variables, the change will be reflected in the datafile and subsequently in the running feature test."
}
[/block]

[block:api-header]
{
  "title": "Launch a feature test"
}
[/block]
You assign metrics, traffic allocation, and optionally audiences and mutually exclusive groups to feature tests; see [Run A/B tests](doc:run-a-b-tests) for more information. After saving your changes, launch your feature test in a [staging environment before using it in production](doc:manage-environments).