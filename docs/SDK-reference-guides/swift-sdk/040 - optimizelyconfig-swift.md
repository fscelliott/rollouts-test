---
title: "OptimizelyConfig"
slug: "optimizelyconfig-swift"
hidden: true
createdAt: "2019-12-12T22:57:36.924Z"
updatedAt: "2020-01-08T20:58:27.713Z"
---
[block:api-header]
{
  "title": "Overview"
}
[/block]
Full Stack SDKs open a well-defined set of public APIs, hiding all implementation details. However, some clients may need access to project configuration data within the "datafile." 

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
      "code": "public func getOptimizelyConfig() throws -> OptimizelyConfig",
      "language": "swift"
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
      "code": "public protocol OptimizelyConfig {\n  let revision: String\n  public let experimentsMap: [String: OptimizelyExperiment]\n  public let featuresMap: [String: OptimizelyFeature]\n}\n\npublic protocol OptimizelyExperiment {\n  let id: String\n  let key: String\n  let variationsMap: [String: OptimizelyVariation]\n}\n\npublic protocol OptimizelyFeature {\n  let id: String\n  let key: String\n  let experimentsMap: [String: OptimizelyExperiment]\n  let variablesMap: [String: OptimizelyVariable]\n}\n\npublic protocol OptimizelyVariation {\n  let id: String\n  let key: String\n  let featureEnabled: Bool?\n  let variablesMap: [String: OptimizelyVariable]\n}\n\npublic protocol OptimizelyVariable {\n  let id: String\n  let key: String\n  let type: String\n  let value: String\n}\n\n",
      "language": "swift"
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
      "code": "let config = try! optimizelyClient.getOptimizelyConfig()\n\n// all experiments\nlet experiments = config.experimentsMap.values\nlet experimentKeys = config.experimentsMap.keys\n\nfor expKey in experimentKeys {\n   let experiment = config.experimentsMap[expKey]\n\n   // all variations for an experiment\n   let variationsMap = experiment.variationsMap\n   let variations = variationsMap.values\n   let variationKeys = variationsMap.keys\n  \n   for varKey in variationKeys {\n      let variation = variationsMap[varKey]\n\n      // all variables for a variation\n  \t\tlet variablesMap = variation.variablesMap\n      let variables = variablesMap.values\n      let variableKeys = variablesMap.keys\n\n      for variableKey in variableKeys {\n         let variable = variablesMap[variableKey]  \n        \n         // use variable data here...\n        \n      }\n   }\n}\n\n// all features\nlet features = config.featuresMap.values\nlet featureKeys = config.featuresMap.keys\n\nfor featKey in featureKeys {\n   let feature = config.featuresMap[featKey]\n\n   // all experiments for a feature\n   let experimentsMap = feature.experimentsMap\n   let experiments = experimentsMap.values\n   let experimentKeys = experimentsMap.keys\n  \n    // use experiments and other feature data here...\n  \n}\n\n// listen to NotificationType.datafileChange to get updated data\n\n_ = optimizely.notificationCenter?.addDatafileChangeNotificationListener { (_) in\n            if let newOptConfig = try? optimizely.getOptimizelyConfig() {\n                print(\"[OptimizelyConfig] revision = \\(newOptConfig.revision)\")\n            }\n        }\n",
      "language": "swift"
    }
  ]
}
[/block]