---
title: "OptimizelyConfig"
slug: "optimizelyconfig-react"
hidden: true
createdAt: "2020-01-17T12:31:02.370Z"
updatedAt: "2020-02-03T19:53:07.516Z"
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
      "code": "OptimizelyConfig getOptimizelyConfig();",
      "language": "typescript"
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
      "code": "// OptimizelyConfig is an object describing the current project configuration data being used by this SDK instance.\nexport interface OptimizelyConfig {\n  experimentsMap: {\n    [experimentKey: string]: {\n      experiment: OptimizelyExperiment;\n    };\n  };\n  featuresMap: {\n    [featureKey: string]: {\n      feature: OptimizelyFeature;\n    };\n  };\n  revision: string;\n}\n\n// OptimizelyFeature is an object describing a feature\nexport interface OptimizelyFeature {\n  id: string;\n  key: string;\n  experimentsMap: {\n    [experimentKey: string]: {\n      experiment: OptimizelyExperiment;\n    };\n  };\n  variablesMap: {\n    [variableKey: string]: {\n      variable: OptimizelyVariable;\n    };\n  };\n}\n\n// OptimizelyExperiment is an object describing a feature test or an A/B test\nexport interface OptimizelyExperiment {\n  id: string;\n  key: string;\n  variationsMap: {\n    [variationKey: string]: {\n      variation: OptimizelyVariation;\n    };\n  };\n}\n\n// OptimizelyVariation is an object describing a variation in a feature test or A/B test\nexport interface OptimizelyVariation {\n  id: string;\n  key: string;\n  variablesMap: {\n    [variableKey: string]: {\n      variable: OptimizelyVariable;\n    };\n  };\n}\n\n// OptimizelyVariable is an object describing a feature variable\nexport interface OptimizelyVariable {\n  id: string;\n  key: string;\n  type: string;\n  value: string;\n}\n",
      "language": "typescript"
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
      "code": "const config = optimizelyClient.getOptimizelyConfig();\n\n// all experiments\nconst experimentsMap = config.experimentsMap;\nconst experiments = Object.values(experimentsMap);\nconst experimentKeys = Object.keys(experimentsMap);\n\nexperimentKeys.forEach(expKey => {\n   const experiment = experimentsMap[expKey]\n\n   // all variations for an experiment\n   const variationsMap = experiment.variationsMap;\n   const variations = Object.values(variationsMap);\n   const variationKeys = Object.keys(variationsMap);\n  \n   variationKeys.forEach(varKey => {\n      var variation = variationsMap[varKey];\n\n      // all variables for a variation\n      const variablesMap = variation.variablesMap;\n      const variables = Object.values(variablesMap);\n      const variableKeys = Object.keys(variablesMap);\n\n      variableKeys.forEach(variableKey => {\n            let variable = variablesMap[variableKey] \n        \n        // use variable data here...\n        \n      });\n   });\n});\n\n// all features\nconst featuresMap = config.featuresMap;\nconst features = Object.values(featuresMap);\nconst featureKeys = Object.keys(featuresMap);\n\nfeatureKeys.forEach(featKey => {\n  const feature = featuresMap[featKey]\n\n  // all experiments for a feature\n  const experimentsMap = feature.experimentsMap\n  const experiments = Object.values(experimentsMap);\n  const experimentKeys = Object.keys(experimentsMap);\n  \n  // use experiments and other feature data here...\n\n});\n\n\n// listen to OPTIMIZELY_CONFIG_UPDATE to get updated data\noptimizelyClient.notificationCenter.addNotificationListener(\n  'OPTIMIZELY_CONFIG_UPDATE',\n  function() {\n    optimizelyClient.OptimizelyConfig newConfig = optimizelyClient.getOptimizelyConfig();\n    // ...\n  }\n);",
      "language": "typescript"
    }
  ]
}
[/block]