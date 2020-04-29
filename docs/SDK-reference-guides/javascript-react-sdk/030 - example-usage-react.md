---
title: "Example usage"
slug: "example-usage-react"
hidden: true
createdAt: "2019-09-11T22:28:34.938Z"
updatedAt: "2019-10-21T21:18:56.361Z"
---
Once you've installed the React SDK, import the Optimizely library into your code, get your Optimizely project's datafile, and instantiate a client. Then, you can use the client to evaluate feature flags, activate an A/B test, or feature test.

This example demonstrates the basic usage of each of these concepts. This example shows how to: 
1. Evaluate a feature with the key `price_filter` and check a configuration variable on it called `min_price`. The SDK evaluates your feature test and rollouts to determine whether the feature is enabled for a particular user, and which minimum price they should see if so.

2. Run an A/B test called `app_redesign`. This experiment has two variations, `control` and `treatment`. It uses the `activate` method to assign the user to a variation, returning its key. As a side effect, the activate function also sends an impression event to Optimizely to record that the current user has been exposed to the experiment. 

3. Use event tracking to track an event called `purchased`. This conversion event measures the impact of an experiment. Using the track method, the purchase is automatically attributed back to the running A/B and feature tests we've activated, and the SDK sends a network request to Optimizely via the customizable event dispatcher so we can count it in your results page.
[block:code]
{
  "codes": [
    {
      "code": "import React from 'react';\nimport {\n  createInstance,\n  OptimizelyProvider,\n  OptimizelyFeature,\n  OptimizelyExperiment,\n  OptimizelyVariation,\n  withOptimizely,\n} from '@optimizely/react-sdk'\n\nconst optimizely = createInstance({\n  sdkKey: '<Your_SDK_Key>', // TODO: Update to your SDK Key\n})\n\nclass Button extends React.Component {\n  onClick = () => {\n    const { optimizely } = this.props\n    optimizely.track('purchased')\n  }\n\n  render() {\n    <button onClick={this.onClick}>\n      Purchase\n    </button>\n  }\n}\n\nconst PurchaseButton = withOptimizely(Button)\n\n\nfunction App() {\n  return (\n    <OptimizelyProvider\n      optimizely={optimizely}\n      user={{\n        id: 'user123',\n      }}\n    >\n      <div className=\"App\">\n        <OptimizelyFeature feature=\"price_filter\">\n          {(isEnabled, variables) => (\n            isEnabled\n            ? `Price filter enabled with a min price of ${variables.min_price}`\n            : `Price filter is NOT enabled`\n          )}\n          <PurchaseButton />\n        </OptimizelyFeature>\n        <OptimizelyExperiment experiment=\"app_redesign\">\n          <OptimizelyVariation variation=\"control\">\n            <h1>Old Site</h1>\n            <PurchaseButton />\n          </OptimizelyVariation>\n\n          <OptimizelyVariation variation=\"treatment\">\n            <h1>Redesigned Site</h1>\n            <PurchaseButton />\n          </OptimizelyVariation>\n        </OptimizelyExperiment>\n      </div>\n    </OptimizelyProvider>\n  );\n}\n\nexport default App;\n\n",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]