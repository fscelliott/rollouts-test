---
title: "Get Feature Variable"
slug: "get-feature-variable-php"
hidden: true
createdAt: "2019-09-12T16:06:00.696Z"
updatedAt: "2019-09-13T12:28:30.177Z"
---
Evaluates the specified feature variable of a specific variable type and returns its value.  

This method is used to evaluate and return a feature variable. Multiple versions of this method are available and are named according to the data type they return:
  * [Boolean](#section-boolean)
  * [Double](#section-double)
  * [Integer](#section-integer)
  * [String](#section-string)

This method takes into account the user `attributes` passed in, to determine if the user is part of the audience that qualifies for the experiment.

### Boolean

Returns the value of the specified Boolean variable.
[block:code]
{
  "codes": [
    {
      "code": "public function getFeatureVariableBoolean($featureFlagKey, $variableKey, $userId, $attributes = null)\n\n",
      "language": "php"
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
      "code": "public function getFeatureVariableDouble($featureFlagKey, $variableKey, $userId, $attributes = null)\n\n",
      "language": "php"
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
      "code": "public function getFeatureVariableInteger($featureFlagKey, $variableKey, $userId, $attributes = null)\n\n",
      "language": "php"
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
      "code": "public function getFeatureVariableString($featureFlagKey, $variableKey, $userId, $attributes = null)\n\n",
      "language": "php"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.0, v3.1
[block:api-header]
{
  "title": "Description"
}
[/block]
Each of the Get Feature Variable methods follows the same logic as [Is Feature Enabled](doc:is-feature-enabled-php):
1. Evaluate any feature tests running for a user.
2. Check the default configuration on a rollout.

The default value is returned if neither of these are applicable for the specified user, or if the user is in a variation where the feature is disabled.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "Unlike [Is Feature Enabled](doc:is-feature-enabled-php), the Get Feature Variable methods do not trigger an impression event. This means that if you're running a feature test, events won't be counted until you call Is Feature Enabled. If you don't call Is Feature Enabled, you won't see any visitors on your results page."
}
[/block]

[block:api-header]
{
  "title": "Parameters"
}
[/block]
Required and optional parameters in Android are listed below.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "h-2": "Description",
    "0-0": "**featureFlagKey**\n*required*",
    "0-1": "string",
    "1-0": "**variableKey**\n*required*",
    "1-1": "string",
    "0-2": "The feature key is defined from the Features dashboard.",
    "1-2": "The key that identifies the feature variable.",
    "2-0": "**userId**\n*required*",
    "3-0": "**userAttributes**\n*required*",
    "2-1": "string",
    "3-1": "map",
    "2-2": "The user ID string uniquely identifies the participant in the experiment.",
    "3-2": "A map of custom key-value string pairs specifying attributes for the user that are used for audience targeting and results segmentation. Non-string values are only supported in the 3.0 SDK and above."
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
@return string variable value / null
[block:api-header]
{
  "title": "Example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "$attributes = [\n  'device' => 'iPhone',\n  'lifetime' => 24738388,\n  'is_logged_in' => true\n];\n\n$featureVariableValue = $optimizelyClient->getFeatureVariableDouble('my_feature_key', 'double_variable_key', 'user_123', $attributes);\n\n",
      "language": "php"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Side effects"
}
[/block]
In SDKs v3.1 and later: Invokes the `DECISION` [notification listener](doc:set-up-notification-listener-php) if this listener is enabled.
[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for PHP is [Optimizely.php](https://github.com/optimizely/php-sdk/blob/master/src/Optimizely/Optimizely.php).