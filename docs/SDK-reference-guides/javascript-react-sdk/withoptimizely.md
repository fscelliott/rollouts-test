---
title: "withOptimizely"
slug: "withoptimizely"
hidden: false
createdAt: "2019-09-27T21:57:56.112Z"
updatedAt: "2019-09-27T22:06:25.400Z"
---
By using the higher-order component (HoC) withOptimizely, any component under the OptimizelyProvider can access the Optimizely ReactSDKClient, which exposes Optimizely information and APIs. 
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
In addition to providing a component with access to all core Optimizely APIs by providing the wrapped component with an `optimizely` prop, the withOptimizely higher order component also makes usage of the APIs more convenient.

The `optimizely` object is automatically associated with the user prop passed to the ancestor OptimizelyProvider. The result is that the id and attributes from that user object will automatically be forwarded to all appropriate SDK method calls. So, there is no need to pass the userId or attributes arguments when calling methods of the `optimizely` client object after using withOptimizely, unless you wish to use different userId or attributes than those given to OptimizelyProvider.
[block:api-header]
{
  "title": "Parameters"
}
[/block]
This table lists the required and optional parameters for the Node SDK.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**Component**",
    "0-1": "React.Component",
    "0-2": "Component which will be enhanced with the following props"
  },
  "cols": 3,
  "rows": 1
}
[/block]

[block:api-header]
{
  "title": "Props Provided"
}
[/block]

[block:parameters]
{
  "data": {
    "h-0": "Prop",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**optimizely**",
    "0-1": "ReactSDKClient",
    "0-2": "The client object which was passed to the OptimizelyProvider",
    "1-0": "**optimizelyReadyTimeout**",
    "1-1": "number | undefined",
    "1-2": "The timeout which was passed to the OptimizelyProvider",
    "2-0": "**isServerSide**",
    "2-1": "boolean",
    "2-2": "Value that was passed to the OptimizelyProvider"
  },
  "cols": 3,
  "rows": 3
}
[/block]

[block:api-header]
{
  "title": "Returns"
}
[/block]
A wrapped component with additional props as described above
[block:api-header]
{
  "title": "Example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "import { withOptimizely } from '@optimizely/react-sdk'\n\nclass MyComp extends React.Component {\n  constructor(props) {\n    super(props)\n    const { optimizely } = this.props\n    const isFeat1Enabled = optimizely.isFeatureEnabled('feat1')\n    const feat1Variables = optimizely.getFeatureVariables('feat1')\n\n    this.state = {\n      isFeat1Enabled,\n      feat1Variables,\n    }\n  }\n\n  render() {\n  }\n}\n\nconst WrappedMyComponent = withOptimizely(MyComp)",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Note",
  "body": "As mentioned above, the `optimizely` client object provided via withOptimizely is automatically associated with the user prop passed to the ancestor OptimizelyProvider. There is no need to pass userId or attributes arguments when calling track, unless you wish to use different userId or attributes than those given to OptimizelyProvider."
}
[/block]

[block:api-header]
{
  "title": "Source"
}
[/block]
The language/platform source files containing the implementation for React is [index.ts](https://github.com/optimizely/react-sdk/blob/master/src/index.ts).