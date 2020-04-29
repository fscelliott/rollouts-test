---
title: "ReactSDKClient"
slug: "reactsdkclient"
hidden: false
createdAt: "2019-09-27T21:58:07.881Z"
updatedAt: "2019-09-27T22:07:19.054Z"
---
ReactSDKClient is an interface that includes APIs to interact with Optimizely. The APIs are described below.
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
An instance of the ReactSDKClient is created with `createInstance` and it is also passed down to child components in the `optimizely` prop with the `withOptimizely` higher-order component.

The ReactSDKClient provides the following API interface below. Note that if the instance was provided by `withOptimizely` or if `setUser` was called, then you do not need to pass in userId or attributes arguments when calling methods of the optimizely client object, unless you wish to use different userId or attributes than those given to `OptimizelyProvider` or `setUser`.
[block:api-header]
{
  "title": "APIs Provided"
}
[/block]
* `onReady(opts?: { timeout?: number }): Promise` Returns a Promise that fulfills with an object representing the datafile fetch process. See [JavaScript: Update datafiles](https://docs.developers.optimizely.com/full-stack/docs/javascript-update-datafiles)
* `user: User` The current user associated with this client instance
* `setUser(userInfo: User): void` Call this to update the current user
* `onUserUpdate(handler: (userInfo: User) => void): () => void` Subscribe a callback to be called when this instance's current user changes. Returns a function that will unsubscribe the callback.
* `getFeatureVariables(featureKey: string, overrideUserId?: string, overrideAttributes?: UserAttributes): VariableValuesObject`: Decide and return variable values for the given feature and user
* `getFeatureVariableString(featureKey: string, variableKey: string, overrideUserId?: string, overrideAttributes?: optimizely.UserAttributes): string | null`: Decide and return the variable value for the given feature, variable, and user
* `getFeatureVariableInteger(featureKey: string, variableKey: string, overrideUserId?: string, overrideAttributes?: UserAttributes): number | null` Decide and return the variable value for the given feature, variable, and user
* `getFeatureVariableBoolean(featureKey: string, variableKey: string, overrideUserId?: string, overrideAttributes?: UserAttributes): boolean | null` Decide and return the variable value for the given feature, variable, and user
* `getFeatureVariableDouble(featureKey: string, variableKey: string, overrideUserId?: string, overrideAttributes?: UserAttributes): number | null` Decide and return the variable value for the given feature, variable, and user
* `isFeatureEnabled(featureKey: string, overrideUserId?: string, overrideAttributes?: UserAttributes): boolean` Return the enabled status for the given feature and user
* `getEnabledFeatures(overrideUserId?: string, overrideAttributes?: UserAttributes): Array<string>`: Return the keys of all features enabled for the given user
[block:api-header]
{
  "title": "Example"
}
[/block]
Example of getting an instance of ReactSDKClient from `createInstance`:
[block:code]
{
  "codes": [
    {
      "code": "import React from 'react';\nimport {\n  createInstance,\n  OptimizelyProvider,\n} from '@optimizely/react-sdk'\n\nconst optimizely = createInstance({\n  sdkKey: 'CxpSJttFVpz8LiLg8jnZwq',\n})\n\noptimizely.setUser({\n  id: 'user123',\n  attributes: {\n    device: 'iPhone',\n    lifetime: 24738388,\n    is_logged_in: true,\n  }\n});\n\nfunction App() {\n  return (\n    <OptimizelyProvider optimizely={optimizely}>\n      <div className=\"App\">Application</div>\n    </OptimizelyProvider>\n  );\n}\n\nexport default App;\n\n\n",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Typescript Types"
}
[/block]
The following type definitions are used in the `ReactSDKClient` interface:

* `UserAttributes : { [name: string]: any }`
* `User : { id: string | null, attributes: userAttributes }`
* `VariableValuesObject : { [key: string]: boolean | number | string | null }`
* `EventTags : { [key: string]: string | number | boolean; }`
[block:api-header]
{
  "title": "Source"
}
[/block]
The language/platform source files containing the implementation for React is [index.ts](https://github.com/optimizely/fullstack-labs/blob/master/packages/react-sdk/src/index.ts).