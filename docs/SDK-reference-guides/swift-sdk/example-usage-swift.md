---
title: "Example usage"
slug: "example-usage-swift"
hidden: false
createdAt: "2019-09-11T22:36:22.529Z"
updatedAt: "2019-09-13T13:02:45.209Z"
---
Once you've installed an SDK, import the Optimizely library into your code, get your Optimizely project's datafile, and instantiate a client. Then, you can use the client to evaluate feature flags, activate an A/B test, or feature test.

This example demonstrates the basic usage of each of these concepts. This example shows how to: 
1. Evaluate a feature with the key `price_filter` and check a configuration variable on it called `min_price`. The SDK evaluates your feature test and rollouts to determine whether the feature is enabled for a particular user, and which minimum price they should see if so.

2. Run an A/B test called `app_redesign`. This experiment has two variations, `control` and `treatment`. It uses the `activate` method to assign the user to a variation, returning its key. As a side effect, the activate function also sends an impression event to Optimizely to record that the current user has been exposed to the experiment. 

3. Use event tracking to track an event called `purchased`. This conversion event measures the impact of an experiment. Using the track method, the purchase is automatically attributed back to the running A/B and feature tests we've activated, and the SDK sends a network request to Optimizely via the customizable event dispatcher so we can count it in your results page.
[block:code]
{
  "codes": [
    {
      "code": "// Evaluate a feature flag and variable\nlet enabled = optimizelyClient.isFeatureEnabled(featureKey: \"price_filter\", \n                                                 userId: userId)\ndo {\n\tlet min_price = try optimizelyClient.getFeatureVariableInteger(featureKey: \"price_filter\", \n                                                            variableKey: \"min_price\", \n                                                            userId: userId)\n} catch {\n  // Execute code for invalid variable value\n}\n\n// Activate an A/B test\ndo {\n\tlet variation = try optimizelyClient.activate(\"app_redesign\", userId:userId)\n  if (variation.variationKey == \"control\") {\n    // Execute code for variation A\n  } else if (variation.variationKey == \"treatment\") {\n    // Execute code for variation B\n  }\n} catch {\n  // Execute code for users who don't qualify for the experiment\n}\n\n// Track an event\ntry? optimizelyClient.track(\"purchased\", userId:userId)\n\n",
      "language": "swift"
    },
    {
      "code": "// Evaluate a feature flag and a variable\nBOOL enabled = [optimizelyClient isFeatureEnabled:@\"price_filter\" userId: userId attributes:nil];\nNSNumber *min_price = [optimizelyClient getFeatureVariableInteger:@\"price_filter\"\n                                               \t\tvariableKey:@\"min_price\"\n                                                  \tuserId:userId\n                                                  \tattributes:nil\n                       \t\t\t\t\t\t\t\t\t\t\t\t\t\t\terror:nil];\n\n// Activate an A/B test\nNSString *variation = [optimizelyClient activate:@\"app_redesign\" \n                       userId:userId\n                       error:nil];\nif ([variation isEqualToString:@\"control\"]) {\n    // Execute code for variation A\n} else if ([variation isEqualToString:@\"treatment\"]) {\n    // Execute code for variation B\n} else {\n    // Execute code for users who don't qualify for the experiment\n}\n\n// Track an event\n[optimizelyClient trackWithEventKey:@\"purchased\"\n                   \t\t\t\t\tuserId:userId\n                   \t\t\t\t\tattributes:nil\n                    \t\t\t\teventTags:nil\n                    \t\t\t\terror:nil];\n",
      "language": "objectivec"
    }
  ]
}
[/block]