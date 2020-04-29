---
title: "OptimizelyVariation"
slug: "optimizelyvariation"
hidden: true
createdAt: "2019-09-25T15:53:38.149Z"
updatedAt: "2019-09-26T18:07:33.071Z"
---
OptimizelyVariation is used with a parent OptimizelyExperiment to render different content for different variations.
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
The OptimizelyExperiment component buckets a user into one of the variations defined on the experiment and passes the `variation_key` of the variation assigned to the children component. The OptimizelyVariation component labeled with the `variation` prop equal to the `variation_key` will be rendered inside the OptimizelyExperiment component.
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
    "0-0": "**variation**",
    "0-1": "string",
    "1-0": "**default**\n_optional_",
    "1-1": "boolean",
    "h-2": "Description",
    "0-2": "The key of the variation for which content should be rendered. Variation keys are in the experiment details page.",
    "1-2": "When true, content will be rendered in the default case (null variation returned from the OptimizelyExperiment)",
    "2-0": "**children**",
    "2-2": "Content or function returning content to be rendered based on the enabled status and variable values of the feature. See usage examples below.",
    "2-1": "React.ReactNode | Function"
  },
  "cols": 3,
  "rows": 3
}
[/block]

[block:api-header]
{
  "title": "Examples"
}
[/block]
If your experiment key was `exp1` and that experiment had a variation `simple` and `detailed`, you could use the ExperimentComponent to render a `SimpleComponent` or a `DetailedComponent` depending on whether the user was assigned the `'simple'` variation or `'detailed`' variation. If for some reason Optimizely did not initialize in the 3000 ms timeout, then the default variation would be rendered instead:
[block:code]
{
  "codes": [
    {
      "code": "import {\n  OptimizelyExperiment,\n  OptimizelyVariation\n} from '@optimizely/react-sdk'\n\nfunction ExperimentComponent() {\n  return (\n    <OptimizelyExperiment experiment=\"exp1\" timeout={3000}>\n      <OptimizelyVariation variation=\"simple\">\n        <SimpleComponent />\n      </OptimizelyVariation>\n\n      <OptimizelyVariation variation=\"detailed\">\n        <ComplexComponent />\n      </OptimizelyVariation>\n\n      <OptimizelyVariation default>\n        <SimpleComponent />\n      </OptimizelyVariation>\n    </OptimizelyExperiment>\n  )\n}",
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