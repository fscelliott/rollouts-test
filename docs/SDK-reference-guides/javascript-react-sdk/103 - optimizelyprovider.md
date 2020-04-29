---
title: "OptimizelyProvider"
slug: "optimizelyprovider"
hidden: false
createdAt: "2019-09-27T21:57:36.687Z"
updatedAt: "2019-09-27T22:01:48.508Z"
---
Wrap your React application at the root with OptimizelyProvider to give your entire React application access to Optimizely's APIs.
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
The OptimizelyProvider leverages React’s Context API to allow access to the ReactSDKClient to components like OptimizelyFeature and OptimizelyExperiment.
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
    "0-0": "**optimizely**",
    "0-1": "ReactSDKClient",
    "h-2": "Description",
    "0-2": "Optimizely instance created from calling `createInstance`",
    "1-0": "**user**",
    "1-1": "object: { id: string; attributes?: { [key: string]: any } }  | Promise User info",
    "1-2": "The user id and user attributes to be passed to the SDK for every feature flag, A/B test, or track call, or a Promise for the same kind of object.",
    "2-0": "**timeout**\n_optional_",
    "2-2": "The amount of time for OptimizelyExperiment and OptimizelyFeature components to render null while waiting for the SDK instance to become ready, before resolving.",
    "2-1": "number",
    "3-0": "**isServerSide**\n_optional_",
    "3-1": "boolean",
    "3-2": "must pass true here for server side rendering"
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
If the root component of your application was <App>, wrap your App with the OptimizelyProvider component. Pass the result of calling `createInstance` to the `optimizely` prop and set the `user` object with `id` and `attributes` identifying the user:
[block:code]
{
  "codes": [
    {
      "code": "import React from 'react';\nimport {\n  createInstance,\n  OptimizelyProvider,\n} from '@optimizely/react-sdk'\n\nconst optimizely = createInstance({\n  sdkKey: '<Your_SDK_Key>',\n})\n\nclass AppWrapper extends React.Component {\n  render() {\n    return (\n      <OptimizelyProvider\n        optimizely={optimizely}\n        user={\n          {\n            id: 'user123',\n            attributes: {\n              'device': 'iPhone',\n              'lifetime': 24738388,\n              'is_logged_in': True,\n            }\n          }\n        }>\n        <App />\n      </OptimizelyProvider>\n    )\n  }",
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