---
title: "OptimizelyConfig"
slug: "optimizelyconfig-javascript-node"
hidden: false
createdAt: "2019-12-13T00:22:53.005Z"
updatedAt: "2020-01-17T19:35:24.447Z"
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
      "code": "var config = optimizelyClient.getOptimizelyConfig();",
      "language": "javascript"
    }
  ]
}
[/block]
`getOptimizelyConfig` returns
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
      "code": "/**\n * OptimizelyConfig is an object describing the current project configuration data being used by this SDK instance.\n * @typedef {string} revision\n * @property {Object.<string, OptimizelyExperiment>}} experimentsMap\n * @property {Object.<string, OptimizelyFeature>}} featuresMap\n */\n\n\n/**\n * OptimizelyFeature is an object describing a feature\n * @typedef {Object} OptimizelyFeature\n * @property {string} id\n * @property {string} key\n * @property {Object.<string, OptimizelyExperiment>}} experimentsMap\n * @property {Object.<string, OptimizelyVariable>}} variablesMap\n */\n\n/**\n * OptimizelyExperiment is an object describing a feature test or an A/B test\n * @typedef {Object} OptimizelyExperiment\n * @property {string} id\n * @property {string} key\n * @property {Object.<string, OptimizelyVariation>}} variationMap\n */\n\n\n/**\n * OptimizelyVariation is an object describing a variation in a feature test or A/B test\n * @typedef {Object} OptimizelyVariation\n * @property {string} id\n * @property {string} key\n * @property {boolean=} featureEnabled\n * @property {Object.<string, OptimizelyVariable>}} variablesMap\n */\n\n/**\n * OptimizelyVariable is an object describing a feature variable\n * @typedef {Object} OptimizelyVariable\n * @property {string} id\n * @property {string} key\n * @property {string} type\n * @property {string} value\n */",
      "language": "javascript"
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
      "code": "var config = optimizelyClient.getOptimizelyConfig();\n\n\n// all experiments\nvar experimentsMap = config.experimentsMap;\nvar experiments = Object.values(experimentsMap);\nvar experimentKeys = Object.keys(experimentsMap);\n\nexperimentKeys.forEach(expKey => {\n   var experiment = experimentsMap[expKey]\n\n   // all variations for an experiment\n   var variationsMap = experiment.variationsMap;\n   var variations = Object.values(variationsMap);\n   var variationKeys = Object.keys(variationsMap);\n  \n   variationKeys.forEach(varKey => {\n      var variation = variationsMap[varKey];\n\n      // all variables for a variation\n  \t\tvar variablesMap = variation.variablesMap;\n      var variables = Object.values(variablesMap);\n      var variableKeys = Object.keys(variablesMap);\n\n      variableKeys.forEach(variableKey => {\n    \t\tlet variable = variablesMap[variableKey] \n        \n        // use variable data here...\n        \n      });\n   });\n});\n\n// all features\nvar featuresMap = config.featuresMap;\nvar features = Object.values(featuresMap);\nlet featureKeys = Object.keys(featuresMap);\n\nfeatureKeys.forEach(featKey => {\n  var feature = featuresMap[featKey]\n\n  // all experiments for a feature\n  var experimentsMap = feature.experimentsMap\n\tvar experiments = Object.values(experimentsMap);\n\tvar experimentKeys = Object.keys(experimentsMap);\n  \n  // use experiments and other feature data here...\n\n});\n\n\n// listen to OPTIMIZELY_CONFIG_UPDATE to get updated data\noptimizelyClient.notificationCenter.addNotificationListener(\n  enums.NOTIFICATION_TYPES.OPTIMIZELY_CONFIG_UPDATE,\n  function() {\n    var newConfig = optimizelyClient.getOptimizelyConfig();\n    // ...\n  }\n);",
      "language": "javascript"
    }
  ]
}
[/block]