---
title: "OptimizelyConfig"
slug: "optimizelyconfig-ruby"
hidden: true
createdAt: "2020-01-17T12:33:55.623Z"
updatedAt: "2020-01-29T19:54:11.461Z"
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
      "code": "def get_optimizely_config",
      "language": "ruby"
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
      "code": "# OptimizelyConfig is a hash describing the current project configuration data\n# being used by this SDK instance. The hash consists of:\n#\n# revision - string\n# experimentsMap - hash of experiment key to OptimizelyExperiment \n# featuresMap - hash of feature key to OptimizelyFeature \n\n# OptimizelyFeature is a hash describing a feature. The hash consists of:\n#\n# id - string\n# key - string\n# experimentsMap - hash of experiment key to OptimizelyExperiment \n# variablesMap - hash of variable key to OptimizelyVariable \n\n\n# OptimizelyExperiment is a hash describing a feature test or an A/B test. The hash consists of:\n#\n# id - string\n# key - string\n# variationMap - hash of variation key to OptimizelyVariation \n\n\n# OptimizelyVariation is a hash describing a variation in a feature test or A/B test. The hash consists of:\n#\n# id - string\n# key - string\n# featureEnabled - bool\n# variablesMap - hash of variable key to OptimizelyVariable \n\n\n# OptimizelyVariable is a hash describing a feature variable. The hash consists of:\n#\n# id - string\n# key - string\n# type - string\n# value - string",
      "language": "ruby"
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
      "code": "config = optimizely_instance.get_optimizely_config\n\n# all experiments\nexperiments_map = config['experimentsMap']\nexperiments = experiments_map.values\nexperiment_keys = experiments_map.keys\n\nexperiment_keys.each do |exp_key|\n  # all variations of an experiment\n  experiment = experiments_map[exp_key]\n  variations_map = experiment['variationsMap']\n  variations = variations_map.values\n  variation_keys = variations_map.keys\n\n  variation_keys.each do |variation_key|\n    variation = variations_map[variation_key]\n    variables_map = variation['variablesMap']\n    variables = variables_map.values\n    variable_keys = variables_map.keys\n\n    variable_keys.each do |variable_key|\n      variable = variables_map[variable_key]\n      # use variable data here...\n    end\n  end\nend\n\n\n# all features\nfeatures_map = config['featuresMap']\nfeatures = features_map.values\nfeature_keys = features_map.keys\n\nfeature_keys.each do |feature_key|\n  feature = features_map[feature_key]\n\n  experiments_map = feature['experimentsMap']\n  experiments = experiments_map.values\n  experiment_keys = experiments_map.keys\n\n  # use experiments and other feature data here...\nend\n\n# listen to OPTIMIZELY_CONFIG_UPDATE to get updated data\noptimizely_instance.notification_center.add_notification_listener( Optimizely::NotificationCenter::NOTIFICATION_TYPES[:OPTIMIZELY_CONFIG_UPDATE]\n) do |*args|\n  config = optimizely_instance.get_optimizely_config\nend\n",
      "language": "ruby"
    }
  ]
}
[/block]