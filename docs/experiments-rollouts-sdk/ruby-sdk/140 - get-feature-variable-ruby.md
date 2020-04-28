---
title: "Get Feature Variable"
slug: "get-feature-variable-ruby"
hidden: true
createdAt: "2019-09-12T16:42:19.233Z"
updatedAt: "2019-12-03T18:53:06.979Z"
---
Evaluates the specified feature variable of a specific variable type and returns its value.  

This method is used to evaluate and return a feature variable. Multiple versions of this method are available and are named according to the data type they return:
  * [Generic](#section-generic) (only v3.3+)
  * [Boolean](#section-boolean)
  * [Double](#section-double)
  * [Integer](#section-integer)
  * [String](#section-string)

This method takes into account the user `attributes` passed in, to determine if the user is part of the audience that qualifies for the experiment.

### Generic

Starting in v3.3.0 of the SDK you can use the generic feature variable accessor. This can return an integer, double, boolean, or string depending on the variable's defined type.
[block:code]
{
  "codes": [
    {
      "code": "def get_feature_variable(feature_flag_key, variable_key, user_id, attributes = nil)\n  ",
      "language": "ruby",
      "name": "Ruby"
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
      "code": "def get_feature_variable_boolean(feature_flag_key, variable_key, user_id, attributes = nil)\n  \n  ",
      "language": "ruby"
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
      "code": "def get_feature_variable_double(feature_flag_key, variable_key, user_id, attributes = nil)\n  \n  ",
      "language": "ruby"
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
      "code": "def get_feature_variable_integer(feature_flag_key, variable_key, user_id, attributes = nil)\n  \n  ",
      "language": "ruby"
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
      "code": "def get_feature_variable_string(feature_flag_key, variable_key, user_id, attributes = nil)\n  \n  ",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Version"
}
[/block]
SDK v3.0, v3.1, v3.2, v3.3
[block:api-header]
{
  "title": "Description"
}
[/block]
Each of the Get Feature Variable methods follows the same logic as [Is Feature Enabled](doc:is-feature-enabled):
1. Evaluate any feature tests running for a user.
2. Check the default configuration on a rollout.

The default value is returned if neither of these are applicable for the specified user, or if the user is in a variation where the feature is disabled.
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "Unlike [Is Feature Enabled](doc:is-feature-enabled), the Get Feature Variable methods do not trigger an impression event. This means that if you're running a feature test, events won't be counted until you call Is Feature Enabled. If you don't call Is Feature Enabled, you won't see any visitors on your results page."
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
    "0-0": "**feature_flag_key**\n*required*",
    "0-1": "string",
    "1-0": "**variable_key**\n*required*",
    "1-1": "string",
    "0-2": "The feature key is defined from the Features dashboard; see [Use feature flags](doc:use-feature-flags).",
    "1-2": "The key that identifies the feature variable. For more information, see: [Define feature variables](doc:define-feature-variables).",
    "2-0": "**user_id**\n*required*",
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
@return [<type specific>] The value of the variable, or `nil` if the feature key is invalid, the variable key is invalid, or there is a mismatch with the type of the variable.
[block:api-header]
{
  "title": "Example"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "feature_variable_value = optimizely_client.get_feature_variable_double('my_feature_key', 'double_variable_key', 'user_123', attributes)\n\n# or in v3.3+ you can use the untyped variable accessor, which will return a double if the variable you defined in the Optimizely Dashboard is of type double\nfeature_variable_value = optimizely_client.get_feature_variable('my_feature_key', 'double_variable_key', 'user_123', attributes)\n",
      "language": "ruby"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "See also"
}
[/block]
 [Is Feature Enabled](doc:is-feature-enabled)
[block:api-header]
{
  "title": "Side effects"
}
[/block]
In SDKs v3.1 and later: Invokes the `DECISION` [notification listener](doc:31-register-the-decision-notification-listener) if this listener is enabled. 
[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for Ruby is [optimizely.rb](https://github.com/optimizely/ruby-sdk/blob/master/lib/optimizely.rb).