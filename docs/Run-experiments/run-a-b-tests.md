---
title: "Run A/B tests"
slug: "run-a-b-tests"
hidden: false
createdAt: "2019-09-11T11:47:39.233Z"
updatedAt: "2019-09-12T18:47:51.860Z"
---
Not all experiments are tied to specific features that you've already flagged in your code. Sometimes, you'll want to run a standalone test to answer a specific question&mdash;which of two (or more) variations performs best? For example, is it more effective to sort the products on a category page by price or category?

These one-off experiments are called **A/B tests**, as opposed to [feature tests](doc:run-feature-tests) that run on features you've already flagged. With A/B tests, you define two or more **variation keys** and then implement a different code path for each variation. From the Optimizely interface, you can determine which users are eligible for the experiment and how to split traffic between the variations, as well as the [metrics](doc:choose-metrics) you'll use to measure each variation's performance. 

### 1. Select A/B Test in your project

In the *Experiments* tab, click _Create New Experiment_ and select _A/B Test_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/afc775c-create-fs.png",
        "create-fs.png",
        1438,
        431,
        "#f6f6fa"
      ],
      "caption": "",
      "border": true
    }
  ]
}
[/block]
### 2. Set an experiment key

Specify an experiment key.
Your experiment key must contain only alphanumeric characters, hyphens, and underscores. The key must also be unique for your Optimizely project so you can correctly disambiguate experiments in your application.

Don’t change the experiment key without making the corresponding change in your code. 

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/e15d545-new-ab-fs.png",
        "new-ab-fs.png",
        808,
        435,
        "#fbfbfb"
      ],
      "caption": "",
      "border": true
    }
  ]
}
[/block]

### 3. Set experiment traffic allocation

The traffic allocation is the fraction of your total traffic to include in the experiment, specified as a percentage. For example, you set an allocation of 50% for an experiment that is triggered when a user does a search. This means:
 - The experiment is triggered when a visitor does a search, but it won’t be triggered for all users. 50% of users who do a search will be in the experiment, but 50% of users who do a search won't.
 - Users who don’t do a search also won't be in the experiment. In other words, the traffic allocation percentage may not apply to all traffic for your application.

For more information, see our KB article [Traffic allocation and distribution](https://help.optimizely.com/Target_Your_Visitors/Traffic_allocation_and_distribution).

Optimizely determines the traffic allocation at the point where you call the Activate method in the SDK.

You can also add your experiment to an [exclusion group](doc:use-mutual-exclusion) at this point.

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/5903003-traffic-allocation-new-fs.png",
        "traffic-allocation-new-fs.png",
        429,
        269,
        "#f7f7f9"
      ],
      "caption": "",
      "border": true
    }
  ]
}
[/block]

### 4. Set variation keys and traffic distribution

Variations are the different code paths you want to experiment on. Enter a unique variation key to identify the variation in the experiment and optionally a short, human-readable description for reporting purposes.

You must specify at least one variation. There’s no limit to how many variations you can create.

By default, variations are given equal traffic distribution. Customize this value for your experiment's requirements.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/f97a186-vars-new-fs.png",
        "vars-new-fs.png",
        790,
        560,
        "#f7f7f9"
      ],
      "caption": "",
      "border": true
    }
  ]
}
[/block]
### 5. (Optional) Add an audience

You can opt to define audiences if you want to show your experiment only to certain groups of users. See [Define attributes](doc:define-attributes) and [Create audiences](doc:create-audiences).

### 6. Add a metric

[Add events](doc:track-events) that you’re tracking with the Optimizely SDKs as metrics to measure impact. Whether you use existing events or create new events to use as metrics, you must add at least one metric to a experiment. To re-order the metrics, click and drag them into place. 
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "The top metric in an experiment is the primary metric. Stats Engine uses the primary metric to determine whether an A/B test wins or loses, overall. Learn about the [strategy behind primary and secondary metrics](https://help.optimizely.com/Ideate_and_Hypothesize/Primary_and_secondary_metrics_and_monitoring_metrics)."
}
[/block]

[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/91188a4-metrics-new-fs.png",
        "metrics-new-fs.png",
        782,
        557,
        "#f1f5fb"
      ],
      "caption": "",
      "border": true
    }
  ]
}
[/block]
### 7. Complete your experiment setup

Click _Create Experiment_ to complete your experiment setup.

### 8. Implement the code sample into your application

Once you've defined an A/B test, you'll see a code sample for implementing it in your application.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/55ca366-exp-code-new-fs.png",
        "exp-code-new-fs.png",
        773,
        486,
        "#f9fafb"
      ],
      "caption": "",
      "border": true
    }
  ]
}
[/block]
For each A/B test, you use the Activate method to decide which variation a user falls into, then use an `if` statement to apply the code for that variation. See the example below.
[block:code]
{
  "codes": [
    {
      "code": "import com.optimizely.ab.android.sdk.OptimizelyClient;\nimport com.optimizely.ab.config.Variation;\n\n// Activate an A/B test\nVariation variation = optimizelyClient.activate(\"app_redesign\", userId);\nif (variation != null) {\n  if (variation.is(\"control\")) {\n    // Execute code for \"control\" variation\n  } else if (variation.is(\"treatment\")) {\n    // Execute code for \"treatment\" variation\n  }\n} else {\n  // Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "using OptimizelySDK;\n\n// Activate an A/B test\nstring userId = \"\";\nvar variation = optimizelyClient.Activate(\"app_redesign\", userId);\nif (variation != null && !string.IsNullOrEmpty(variation.Key))\n{\n  if (variation.Key == \"control\")\n  {\n    // Execute code for variation A\n  }\n  else if (variation.Key == \"treatment\")\n  {\n    // Execute code for variation B\n  }\n}\nelse\n{\n  // Execute code for your users who don’t qualify for the experiment\n}\n\n",
      "language": "csharp"
    },
    {
      "code": "import com.optimizely.ab.Optimizely;\nimport com.optimizely.ab.config.Variation;\n\n// Activate an A/B test\nVariation variation = optimizelyClient.activate(\"app_redesign\", userId);\nif (variation != null) {\n  if (variation.is(\"control\")) {\n    // Execute code for \"control\" variation\n  } else if (variation.is(\"treatment\")) {\n    // Execute code for \"treatment\" variation\n  }\n} else {\n  // Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "java"
    },
    {
      "code": "// Activate an A/B test\nvar variation = optimizelyClientInstance.activate('app_redesign', userId);\nif (variation === 'control') {\n  // Execute code for \"control\" variation\n} else if (variation === 'treatment') {\n  // Execute code for \"treatment\" variation\n} else {\n  // Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "javascript"
    },
    {
      "code": "// Activate an A/B test\nvar variation = optimizelyClient.activate(\"app_redesign\", userId);\nconsole.log(`User ${userId} is in variation: ${variation}`);\nif (variation === \"control\") {\n  // Execute code for \"control\" variation\n} else if (variation === \"treatment\") {\n  // Execute code for \"treatment\" variation\n} else {\n  // Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "#import \"OPTLYVariation.h\"\n\n// Activate an A/B test\nNSString *variationKey = [optimizely activateWithExperimentKey:@\"app_redesign\"\n                                                        userId:@\"12122\"\n                                                         error:nil];\nif ([variationKey isEqualToString:@\"control\"]) {\n    // Execute code for \"control\" variation\n} else if ([variationKey isEqualToString:@\"treatment\"]) {\n    // Execute code for \"treatment\" variation\n} else {\n    // Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "objectivec"
    },
    {
      "code": "// Activate an A/B test\n$variation = $optimizelyClient->activate('app_redesign', $userId);\nif ($variation == 'control') {\n  // Execute code for \"control\" variation\n} else if ($variation == 'treatment') {\n  // Execute code for \"treatment\" variation\n} else {\n  // Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "php"
    },
    {
      "code": "# Activate an A/B test\nvariation = optimizely_client.activate('app_redesign', user_id)\nif variation == 'control':\n    pass\n    # Execute code for \"control\" variation\nelif variation == 'treatment':\n    pass\n    # Execute code for \"treatment\" variation\nelse:\n    pass\n    # Execute code for users who don't qualify for the experiment\n    \n    ",
      "language": "python"
    },
    {
      "code": "# Activate an A/B test\nvariation = optimizely_client.activate('app_redesign', user_id)\nif variation == 'control'\n  # Execute code for \"control\" variation\nelsif variation == 'treatment'\n  # Execute code for \"treatment\" variation\nelse\n  # Execute code for users who don't qualify for the experiment\nend\n\n",
      "language": "ruby"
    },
    {
      "code": "// Activate an A/B test\nif let variationKey = try? optimizely.activate(experimentKey: \"app_redesign\",\n                                                      userId: \"12122\") {\n\tif variationKey == \"control\" {\n\t\t// Execute code for \"control\" variation\n\t} else if variationKey == \"treatment\" {\n\t\t// Execute code for \"treatment\" variation\n\t}\n  \n} else {\n\t// Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "swift"
    }
  ]
}
[/block]
The Activate method:
 - Evaluates whether the user is eligible for the experiment and returns a 
variation key if so. For more on how the variation is chosen, see [How bucketing works](doc:how-bucketing-works) and the SDK reference guide for the Activate method. 
 - Sends an event to Optimizely to record that the current user has been exposed to the A/B test. You should call Activate at the point you want to record an A/B test exposure to Optimizely. If you don't want to record an A/B test exposure, use the Get Variation method instead.
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "If any of the conditions for the experiment aren't met, the response is `null`. Make sure that your code adequately handles this default case. In general, you'll want to run the baseline experience."
}
[/block]