---
title: "OptimizelyConfig"
slug: "optimizelyconfig-python"
hidden: true
createdAt: "2020-04-28T14:15:09.730Z"
updatedAt: "2020-04-28T14:15:09.730Z"
---
[block:api-header]
{
  "title": "Overview"
}
[/block]
Full Stack SDKs open a well-defined set of public APIs, hiding all implementation details. However, some clients may need access to project configuration data within the "datafile". 

In this document, we extend our public APIs to define data models and access methods, which clients can use to access project configuration data. 

[block:api-header]
{
  "title": "OptimizelyConfig API"
}
[/block]

A public configuration data model (OptimizelyConfig) is defined below as a structured format of static Optimizely Project data.

OptimizelyConfig can be accessed from OptimizelyClient (top-level) with this public API call:
[block:code]
{
  "codes": [
    {
      "code": "def get_optimizely_config(self)",
      "language": "python"
    }
  ]
}
[/block]
`get_optimizely_config` returns
an `OptimizelyConfig` instance which include a datafile revision number, all experiments, and feature flags mapped by their key values.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "When the SDK datafile is updated (the client can add a notification listener for `OPTIMIZELY_CONFIG_UPDATE` to get notified), the client is expected to call the method to get the updated OptimizelyConfig data. See examples below."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "# OptimizelyConfig is an object describing the current project configuration data being used by this SDK instance.\nclass OptimizelyConfig(object):\n    def __init__(self, revision, experiments_map, features_map):\n        self.revision = revision\n        self.experiments_map = experiments_map\n        self.features_map = features_map\n\n# OptimizelyFeature is an object describing a feature.\nclass OptimizelyFeature(object):\n    def __init__(self, id, key, experiments_map, variables_map):\n        self.id = id\n        self.key = key\n        self.experiments_map = experiments_map\n        self.variables_map = variables_map\n\n# OptimizelyExperiment is an object describing a feature test or an A/B test.\nclass OptimizelyExperiment(object):\n    def __init__(self, id, key, variations_map):\n        self.id = id\n        self.key = key\n        self.variations_map = variations_map\n\n# OptimizelyVariation is an object describing a variation in a feature test or A/B test.\nclass OptimizelyVariation(object):\n    def __init__(self, id, key, feature_enabled, variables_map):\n        self.id = id\n        self.key = key\n        self.feature_enabled = feature_enabled\n        self.variables_map = variables_map\n\n# OptimizelyVariable is an object describing a feature variable\nclass OptimizelyVariable(object):\n    def __init__(self, id, key, variable_type, value):\n        self.id = id\n        self.key = key\n        self.type = variable_type\n        self.value = value",
      "language": "python"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Examples"
}
[/block]
OptimizelyConfig can be accessed from OptimizelyClient (top-level) like this:

[block:code]
{
  "codes": [
    {
      "code": "config = optimizely_client.get_optimizely_config()\n\n# all experiments\nexperiments_map = config.experiments_map\nfor experiment_key in experiments_map.keys():\n    # all variations of an experiment\n    variations_map = experiments_map[experiment_key].variations_map\n    for variation_key in variations_map.keys():\n        # all variables in a a variation\n        variables_map = variations_map[variation_key].variables_map\n        for variable_key in variables_map.keys():\n            pass # use variable data here...\n\n\n# all features\nfeatures_map = config.features_map\nfor feature_key in features_map.keys():\n    # all experiments in a feature\n    experiments_map = features_map[feature_key].experiments_map\n    for experiment_key in experiments_map.keys():\n        pass # use experiments and other feature data here...\n\n\n# listen to OPTIMIZELY_CONFIG_UPDATE to get updated data\ndef on_config_update_listener(*args):\n    config = optimizely_client.get_optimizely_config()\n\noptimizely_client.notification_center.add_notification_listener(\n    enums.NotificationTypes.OPTIMIZELY_CONFIG_UPDATE, on_config_update_listener)\n",
      "language": "python"
    }
  ]
}
[/block]