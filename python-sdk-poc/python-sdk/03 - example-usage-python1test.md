---
title: "Example usage"
slug: "example-usage-python"
hidden: false
createdAt: "2019-09-11T22:35:46.640Z"
updatedAt: "2019-09-13T12:41:04.573Z"
---
Once you've installed the Python SDK, import the Optimizely library into your code, get your Optimizely project's datafile, and instantiate a client. Then, you can use the client to evaluate feature flags, activate an A/B test, or feature test.

This example demonstrates the basic usage of each of these concepts. This example shows how to: 
1. Evaluate a feature with the key `price_filter` and check a configuration variable on it called `min_price`. The SDK evaluates your feature test and rollouts to determine whether the feature is enabled for a particular user, and which minimum price they should see if so.

2. Run an A/B test called `app_redesign`. This experiment has two variations, `control` and `treatment`. It uses the `activate` method to assign the user to a variation, returning its key. As a side effect, the activate function also sends an impression event to Optimizely to record that the current user has been exposed to the experiment. 

3. Use event tracking to track an event called `purchased`. This conversion event measures the impact of an experiment. Using the track method, the purchase is automatically attributed back to the running A/B and feature tests we've activated, and the SDK sends a network request to Optimizely via the customizable event dispatcher so we can count it in your results page.
[block:code]
{
  "codes": [
    {
      "code": "from optimizely import optimizely\n\n# Instantiate an Optimizely client\noptimizely_client = optimizely.Optimizely(datafile)\n\n# Evaluate a feature flag and variable\nenabled = optimizely_client.is_feature_enabled('price_filter', user_id)\nmin_price = optimizely_client.get_feature_variable_integer('price_filter', 'min_price', user_id)\n\n# Activate an A/B test\nvariation = optimizely_client.activate('app_redesign', user_id)\nif variation == 'control':\n    pass\n    # Execute code for variation A\nelif variation == 'treatment':\n    pass\n    # Execute code for variation B\nelse:\n    pass\n    # Execute code for users who don't qualify for the experiment\n\n# Track an event\noptimizely_client.track('purchased', user_id)\n\n",
      "language": "python"
    }
  ]
}
[/block]