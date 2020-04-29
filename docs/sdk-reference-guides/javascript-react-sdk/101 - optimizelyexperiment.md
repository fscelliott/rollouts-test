---
title: "OptimizelyExperiment"
slug: "optimizelyexperiment"
hidden: true
createdAt: "2019-09-25T15:40:25.875Z"
updatedAt: "2019-09-26T18:07:20.313Z"
---
The OptimizelyExperiment component allows you to evaluate which variation a user gets in an experiment.
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
The OptimizelyExperiment component buckets a user into one of the variations defined on the experiment and passes the `variation_key` of the variation assigned to the children component so that the application can render different content based on the variation assigned.
[block:api-header]
{
  "title": "Props"
}
[/block]
The table below lists the required and optional props for the OptimizelyExperiment Component in React.
[block:parameters]
{
  "data": {
    "h-0": "Parameter",
    "h-1": "Type",
    "0-0": "**experiment**",
    "0-1": "string",
    "1-0": "**autoUpdate**\n_optional_",
    "1-1": "boolean",
    "h-2": "Description",
    "0-2": "The key of the experiment to check. \nThe experiment key is defined from the Experiments dashboard.",
    "1-2": "If true, this component will re-render in response to datafile or user changes. Default: false.",
    "2-0": "**timeout**\n_optional_",
    "2-1": "number",
    "2-2": "Rendering timeout in milliseconds. If Optimizely has not initialized before this timeout, the OptimizelyExperiment renders with a null variation assignment. Overrides any timeout set on the ancestor OptimizelyProvider.",
    "3-0": "**children**",
    "3-2": "Content or function returning content to be rendered based on the enabled status and variable values of the feature. See usage examples below.",
    "3-1": "React.ReactNode | Function\n\nwith props:\nvariation (string) indicating which variation key was assigned to the user."
  },
  "cols": 3,
  "rows": 4
}
[/block]

[block:api-header]
{
  "title": "Examples"
}
[/block]
You can use OptimizelyExperiment via a child render function. If the component contains a function as a child, OptimizelyExperiment will call that function, with the result of optimizely.activate(experimentKey).

For example: If your experiment key was `exp1` and that experiment had a variation `simple`, you could use the ExperimentComponent to render a `SimpleComponent` or a `DetailedComponent` depending on whether the user was assigned the `'simple'` variation:
[block:code]
{
  "codes": [
    {
      "code": "import { OptimizelyExperiment } from '@optimizely/react-sdk'\n\nfunction ExperimentComponent() {\n  return (\n    <OptimizelyExperiment experiment=\"exp1\">\n      {(variation) => (\n        variation === 'simple'\n          ? <SimpleComponent />\n          : <DetailedComponent />\n      )}\n    </OptimizelyExperiment>\n  )\n}",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]
You can also use the OptimizelyVariation component (see below):


[block:code]
{
  "codes": [
    {
      "code": "import {\n  OptimizelyExperiment,\n  OptimizelyVariation\n} from '@optimizely/react-sdk'\n\nfunction ExperimentComponent() {\n  return (\n    <OptimizelyExperiment experiment=\"exp1\">\n      <OptimizelyVariation variation=\"simple\">\n        <SimpleComponent />\n      </OptimizelyVariation>\n\n      <OptimizelyVariation variation=\"detailed\">\n        <ComplexComponent />\n      </OptimizelyVariation>\n\n      <OptimizelyVariation default>\n        <SimpleComponent />\n      </OptimizelyVariation>\n    </OptimizelyExperiment>\n  )\n}",
      "language": "javascript",
      "name": "React"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Source files"
}
[/block]
The language/platform source files containing the implementation for React is [index.ts](https://github.com/optimizely/react-sdk/blob/master/src/index.ts).