---
title: "OptimizelyFeature"
slug: "optimizelyfeature"
hidden: false
createdAt: "2019-09-27T21:57:46.658Z"
updatedAt: "2019-09-27T22:02:06.031Z"
---
The OptimizelyFeature component allows you to evaluate the state of a feature flag and its variables for a given user. The purpose of this component is to allow you to separate the process of developing and deploying features from the decision to turn on a feature. Build your feature and deploy it to your application behind this component, then turn the feature on or off for specific users.
[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v1.0.0
[block:api-header]
{
  "title": "Description"
}
[/block]
The OptimizelyFeature component traverses the client's datafile and checks the feature flag for the feature key that you specify. The component then evaluates the feature rollout for a user and checks whether the rollout is enabled based on the appropriate traffic allocation and audience targeting.

The component passes a boolean prop, `isEnabled`, to its children indicating whether the feature is enable as well as an object prop, `variables`, indicating the feature variable values associated with the feature.
[block:api-header]
{
  "title": "Props"
}
[/block]
The table below lists the required and optional props for the OptimizelyFeature Component in React.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "0-0": "**feature**",
    "0-1": "string",
    "1-0": "**autoUpdate**\n_optional_",
    "1-1": "boolean",
    "h-2": "Description",
    "0-2": "The key of the feature to check. \nThe feature key is defined from the Features dashboard.",
    "1-2": "If true, this component will re-render in response to datafile or user changes. Default: false.",
    "2-0": "**timeout**\n_optional_",
    "2-1": "number",
    "2-2": "Rendering timeout in milliseconds. If Optimizely has not initialized before this timeout, the OptimizelyFeature will render with `isEnabled` as `false` and the `variables` as their default values. This timeout overrides any timeout set on the ancestor OptimizelyProvider.",
    "3-0": "**children**",
    "3-2": "Content or function returning content to be rendered based on the enabled status and variable values of the feature. See example usage below.",
    "3-1": "React.ReactNode | Function\nwith props:\n`isEnabled` (boolean) indicating whether the feature is enabled or not. \n`variables` (object) mapping feature variable keys to feature variable values."
  },
  "cols": 3,
  "rows": 4
}
[/block]

[block:api-header]
{
  "title": "Examples"
}
[/block]
Import the `OptimizelyFeature` component from the React SDK.
[block:code]
{
  "codes": [
    {
      "code": "import { OptimizelyFeature } from '@optimizely/react-sdk'\n\n",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]
Find a component you would like to control with a feature flag and wrap it in the OptimizelyFeature component.

If your feature key was `'hello_world'` then you could pass the `'hello_world'` string as the feature prop and use the feature component to render different content and different variables like the following:
[block:code]
{
  "codes": [
    {
      "code": "<OptimizelyFeature feature=\"hello_world\">\n  {(isEnabled, variables) => (\n    isEnabled\n    ? (<p> Feature enabled! Variable values: { JSON.stringify(variables) } </p>)\n    : (<p> Feature not enabled. Variable values: { JSON.stringify(variables) } </p>)\n  )}\n</OptimizelyFeature>\n",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for React is [index.ts](https://github.com/optimizely/react-sdk/blob/master/src/index.ts).