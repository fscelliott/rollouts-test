---
title: "Get Feature Variable"
slug: "get-feature-variable-javascript-node"
hidden: false
createdAt: "2019-09-12T15:00:23.402Z"
updatedAt: "2019-12-03T19:41:31.326Z"
---
Evaluates the specified feature variable of a specific variable type and returns its value.  

This method is used to evaluate and return a feature variable. Multiple versions of this method are available and are named according to the data type they return:
  * [Generic](#section-generic)  (only v3.3+)
  * [Boolean](#section-boolean)
  * [Double](#section-double)
  * [Integer](#section-integer)
  * [String](#section-string)

This method takes into account the user `attributes` passed in, to determine if the user is part of the audience that qualifies for the experiment.

### Generic

Starting in v3.3.0 of the SDK, you can use the generic feature variable accessor. This can return a number, boolean, or string depending on the variable's defined type.
[block:code]
{
  "codes": [
    {
      "code": "function getFeatureVariable(featureKey, variableKey, userId, attributes)",
      "language": "javascript"
    }
  ]
}
[/block]
### Boolean

Returns the value of the specified Boolean variable.
[block:code]
{
  "codes": [
    {
      "code": "function getFeatureVariableBoolean(featureKey, variableKey, userId, attributes)\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]
### Double

Returns the value of the specified double variable.
[block:code]
{
  "codes": [
    {
      "code": "function getFeatureVariableDouble(featureKey, variableKey, userId, attributes)\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]
### Integer

Returns the value of the specified integer variable.
[block:code]
{
  "codes": [
    {
      "code": "function getFeatureVariableInteger(featureKey, variableKey, userId, attributes)\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]
### String

Returns the value of the specified string variable.
[block:code]
{
  "codes": [
    {
      "code": "function getFeatureVariableString(featureKey, variableKey, userId, attributes)\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.0, v3.1, v3.3
[block:api-header]
{
  "title": "Description"
}
[/block]
Each of the Get Feature Variable methods follows the same logic as [Is Feature Enabled](doc:is-feature-enabled-javascript-node):
1. Evaluate any feature tests running for a user.
2. Check the default configuration on a rollout.

The default value is returned if neither of these are applicable for the specified user, or if the user is in a variation where the feature is disabled.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "Unlike [Is Feature Enabled](doc:is-feature-enabled-javascript-node), the Get Feature Variable methods do not trigger an impression event. This means that if you're running a feature test, events won't be counted until you call Is Feature Enabled. If you don't call Is Feature Enabled, you won't see any visitors on your results page."
}
[/block]

[block:api-header]
{
  "title": "Parameters"
}
[/block]
Required and optional parameters are listed below.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**featureKey**\n*required*",
    "0-1": "string",
    "1-0": "**variableKey**\n*required*",
    "1-1": "string",
    "0-2": "The feature key is defined from the Features dashboard; see [Use feature flags](doc:use-feature-flags).",
    "1-2": "The key that identifies the feature variable. For more information, see: [Define feature variables](doc:define-feature-variables).",
    "2-0": "**userId**\n*required*",
    "3-0": "**attributes**\n*required*",
    "2-1": "string",
    "3-1": "map",
    "2-2": "The user ID string uniquely identifies the participant in the experiment. For more information, see: [Identify users](doc:identify-users).",
    "3-2": "A map of custom key-value string pairs specifying attributes for the user that are used for [audience targeting](doc:define-audiences-and-attributes) and [results segmentation](doc:analyze-results#section-segment-results). Non-string values are only supported in the 3.0 SDK and above."
  },
  "cols": 3,
  "rows": 4
}
[/block]

[block:api-header]
{
  "title": "Returns"
}
[/block]
@return {string|number|boolean|null} The value of the variable, or `null` if the feature key is invalid, the variable key is invalid, or there is a mismatch with the type of the variable.
[block:api-header]
{
  "title": "Example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "var attributes = {\n  device: 'iPhone',\n  lifetime: 24738388,\n  is_logged_in: true,\n};\n\nvar featureVariableValue = optimizelyClient.getFeatureVariableDouble('my_feature_key', 'double_variable_key', 'user_123', attributes);\n\n// or in v3.3+ you can use the untyped variable accessor, which will return a number if the variable you defined in the Optimizely Dashboard is of type double\nvar featureVariableValue = optimizelyClient.getFeatureVariable('my_feature_key', 'double_variable_key', 'user_123', attributes);\n\n\n",
      "language": "javascript",
      "name": "Node"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "See also"
}
[/block]
 [Is Feature Enabled](doc:is-feature-enabled-javascript-node)
[block:api-header]
{
  "title": "Side effects"
}
[/block]
In SDKs v3.1 and later: Invokes the `DECISION` [notification listener](set-up-notification-listener-javascript-node) if this listener is enabled. 
[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Node is [index.js](https://github.com/optimizely/node-sdk/blob/master/lib/optimizely/index.js).