---
title: "Handle user IDs"
slug: "identify-users"
hidden: false
createdAt: "2019-09-11T11:47:39.213Z"
updatedAt: "2019-09-12T17:42:10.518Z"
---
*User IDs* are used to uniquely identify the participants in your experiments. Supply any string you want for user IDs, depending on your test design.

For example, if you're running tests on anonymous users, you can use a first-party cookie or device ID to identify each participant. If you're running tests on known users, you can use a universal user identifier (UUID) to identify each participant. If you're using UUIDs, you can run tests that span multiple devices or applications and ensure that users have a consistent treatment.

User IDs don't necessarily need to correspond to individual users. If you're running tests in a business application, you may want to pass account IDs to the SDK to ensure that every user in a given account has the same treatment. Alternatively, you can use [bucketing IDs](doc:how-bucketing-works) to ensure these users get a consistent experience.

Here are more tips for using user IDs:

* *Ensure user IDs are unique*: User IDs must be unique among the population you are using for tests. Optimizely buckets users and provides test metrics based on the user IDs that you provide.

* *Anonymize user IDs*: The user IDs you provide are sent to Optimizely servers exactly as you provide them. You are responsible for anonymizing any personally identifiable data such as email addresses in accordance with your company's policies.

* *Use IDs from third-party platforms*: If you are measuring the impact of your tests in a third-party analytics platform, we recommend leveraging the same user ID from that platform. This helps ensure data consistency and make it easier to reconcile events between systems.

* *Use one namespace per project*: Optimizely generally assumes a single namespace of user IDs for each project. If you are using multiple different conventions for user IDs in a single project (for example, anonymous visitor IDs for some tests and UUIDs for others), Optimizely will be unable to enforce rules such as mutual exclusivity between tests.

* *Use either logged-out or logged-in IDs*: Optimizely doesn't currently provide a mechanism to alias logged-out IDs with logged-in IDs. If you are running tests that span both logged-out and logged-in states (for example, test on a signup funnel and track conversions after the user has logged in), you must persist logged-out IDs for the lifetime of the test.
[block:api-header]
{
  "title": "Assign variations with bucketing IDs"
}
[/block]

[block:callout]
{
  "type": "warning",
  "body": "Bucketing IDs are a beta feature intended to support customers who want to assign variations with a different identifier than they use to count visitors. For example, a company might want to assign variations by account ID while counting visitors by user ID. We're investigating the implications of bucketing IDs on results analysis, and we'd love to have your feedback! If you want to participate in this beta release, please [contact support](https://help.optimizely.com/Account_Settings/File_online_tickets_for_support).",
  "title": "BETA"
}
[/block]
By default, Optimizely assigns users to variations (in other words, [Optimizely _buckets_ users](https://www.optimizely.com/optimization-glossary/bucket-testing/)) based on submitted user IDs. You can change this behavior by including a bucketing ID. 

With a bucketing ID, you decouple user bucketing from user identification. Users who have the same bucketing ID are put into the same bucket and are exposed to the same variation. The bucketing ID serves as the seed of the hash value used to assign the user to a variation. Users with the same bucketing ID share the same hash value, which is why they are exposed to the same variation. For more information, see [How bucketing works](doc:how-bucketing-works).

Bucketing IDs are implemented as a reserved [attribute](doc:define-attributes) that you can use when activating an experiment or evaluating a feature flag. The example shows how to include a bucketing ID&mdash;send the `$opt_bucketing_id` attribute in the `attributes` parameter of the Activate, Is Feature Enabled, Get Feature Variable, and Track methods.
[block:code]
{
  "codes": [
    {
      "code": "import java.util.UUID;\n\nimport com.optimizely.ab.android.sdk.OptimizelyClient;\nimport com.optimizely.ab.config.Variation;\n\nString experiementKey = \"my_experiment\";\n// for simplicity sake, we are just using a generated user id\nString userId = UUID.randomUUID().toString();\n\nMap<String,String> attributes = new HashMap<String,String>();\nattributes.put(\"ad_source\", \"my_campaign\");\nattributes.put(\"browser\", \"chrome\");\n// bucketing id can be passed in alone or with other attributes\nattributes.put(\"$opt_bucketing_id\", \"bucketingId123\");\n\n// Activate with bucketing ID\nVariation backgroundVariation = optimizely.activate(experiementKey, userId, attributes);\n\n// Track with bucketing ID\n// This tracks a conversion event for the event named `sample_conversion`\noptimizely.track(\"sample_conversion\", userId, attributes);\n\n// GetVariation with bucketing ID\nbackgroundVariation = optimizely.getVariation(experimentKey, userId, attributes);\n\n",
      "language": "java",
      "name": "Android"
    },
    {
      "code": "using OptimizelySDK;\nusing OptimizelySDK.Entity;\n\nvar optimizelyClient = new Optimizely(datafile);\n\nvar experimentKey = \"my_experiment\";\nvar userId = \"user123\";\nUserAttributes attributes = new UserAttributes\n{\n  { \"DEVICE\", \"iPhone\" },\n  { \"AD_SOURCE\", \"my_campaign\" },\n  { \"$opt_bucketing_id\", \"bucketingId123\" }\n};\n\n// Activate with the bucketing ID\nvar variation = optimizelyClient.Activate(experimentKey, userId, attributes);\n\n// Track with the bucketing ID\nvar eventKey = \"my_conversion\";\noptimizelyClient.Track(eventKey, userId, attributes);\n\n// Get variation with the bucketing ID\nvar variation2 = optimizelyClient.GetVariation(experimentKey, userId, attributes);\n\n",
      "language": "csharp"
    },
    {
      "code": "import java.util.UUID;\n\nimport com.optimizely.ab.Optimizely;\nimport com.optimizely.ab.config.Variation;\n\nString experiementKey = \"my_experiment\";\n// for simplicity sake, we are just using a generated user id\nString userId = UUID.randomUUID().toString();\n\nMap<String,String> attributes = new HashMap<String,String>();\nattributes.put(\"ad_source\", \"my_campaign\");\nattributes.put(\"browser\", \"chrome\");\n// bucketing id can be passed in alone or with other attributes\nattributes.put(\"$opt_bucketing_id\", \"bucketingId123\");\n\n// Activate with bucketing ID\nVariation backgroundVariation = optimizelyClient.activate(experiementKey, userId, attributes);\n\n// Track with bucketing ID\n// This tracks a conversion event for the event named `sample_conversion`\noptimizelyClient.track(\"sample_conversion\", userId, attributes);\n\n// GetVariation with bucketing ID\nbackgroundVariation = optimizelyClient.getVariation(experimentKey, userId, attributes);\n\n",
      "language": "java"
    },
    {
      "code": "var experimentKey = 'my_experiment';\nvar userId = 'user123';\nvar attributes = {\n    'device': 'iphone',\n    'ad_source': 'my_campaign',\n    '$opt_bucketing_id': 'bucketingId123'\n};\n\n// Activate with the bucketing ID\nvar variationKey = optimizelyClient.activate(experimentKey, userId, attributes);\n\n// Track with the bucketing ID\nvar eventkey = 'my_conversion';\noptimizelyClient.track(eventKey, userId, attributes);\n\n// Get variation with the bucketing ID\nvariationKey = optimizelyClient.getVariation(experimentKey, userId, attributes);\n\n",
      "language": "javascript"
    },
    {
      "code": "const bucketingIdAttributes = {\n  device: \"iphone\",\n  ad_source: \"my_campaign\",\n  $opt_bucketing_id: \"bucketingId123\"\n};\n\n// Activate with the bucketing ID\nvar variationKey = optimizelyClient.activate(\n  \"my_experiment\",\n  userId,\n  bucketingIdAttributes\n);\n\n// Track with the bucketing ID\nvar eventKey = \"my_conversion\";\noptimizelyClient.track(eventKey, userId, bucketingIdAttributes);\n\n// Get variation with the bucketing ID\nvariationKey = optimizelyClient.getVariation(\n  \"my_experiment\",\n  userId,\n  bucketingIdAttributes\n);\n\n",
      "language": "javascript",
      "name": "Node"
    },
    {
      "code": "NSString *experimentKey = @\"myExperiment\";\nNSString *userId = @\"user123\";\nNSDictionary *attributes = @{@\"device\"              : @\"iPhone\",\n                             @\"ad_source\"           : @\"my_campaign\",\n                             @\"$opt_bucketing_id\"   : @\"bucketingId123\"};\n\n// Activate with the bucketing ID\nNSString *variationKey = [optimizely activateWithExperimentKey:experimentKey\n                                                    userId:userId\n                                                    attributes:attributes\n                                                    error:nil];\n\n// Track with the bucketing ID\nNSString *eventKey = @\"myEvent\";\n[optimizely trackWithEventKey:eventKey\n                     \t\tuserId:userId\n                       \tattributes:attributes\n                        error:nil];\n\n// Get variation with the bucketing ID\nNSString *variationKey = [optimizely getVariationKeyWithExperimentKey: experimentKey\n                          userId:userId\n                          attributes:attributes\n                          error:nil];\n\n",
      "language": "objectivec"
    },
    {
      "code": "$experimentKey = 'my_experiment';\n$userId = 'user123';\n$attributes = [\n    'device' => 'iphone',\n    'ad_source' => 'my_campaign',\n    '$opt_bucketing_id' => 'bucketingId123'\n];\n\n// Activate with the bucketing ID\n$variationKey = $optimizelyClient->activate($experimentKey, $userId, $attributes);\n\n// Track with the bucketing ID\n$eventKey = 'my_conversion';\n$optimizelyClient->track($eventKey, $userId, $attributes);\n\n// Get variation with the bucketing ID\n$variationKey = $optimizelyClient->getVariation($experimentKey, $userId, $attributes);\n\n",
      "language": "php"
    },
    {
      "code": "experiment_key = 'app_redesign_attrib'\nuser_id = 'user123'\nattributes = {\n    'plan_type': 'silver',\n    '$opt_bucketing_id': 'bucketingId123'\n}\n\n# Activate with the bucketing ID\nvariation = optimizely_client.activate(experiment_key, user_id, attributes)\n\n# Track with the bucketing ID\nevent_key = 'purchased'\noptimizely_client.track(event_key, user_id, attributes)\n\n# Get variation with the bucketing ID\nvariation = optimizely_client.get_variation(experiment_key, user_id, attributes)\n\n",
      "language": "python"
    },
    {
      "code": "experiment_key = 'my_experiment'\nuser_id = 'user123'\nattributes = {\n    'device' => 'iphone',\n    'ad_source' => 'my_campaign',\n    '$opt_bucketing_id' => 'bucketingId123'\n};\n\n# Activate with the bucketing ID\nvariation_key = optimizely_client.activate(experiment_key, user_id, attributes)\n\n# Track with the bucketing ID\nevent_key = 'my_conversion'\noptimizely_client.track(event_key, user_id, attributes)\n\n# Get variation with the bucketing ID\nvariation_key = optimizely_client.get_variation(experiment_key, user_id, attributes)\n\n",
      "language": "ruby"
    },
    {
      "code": "let experimentKey = \"myExperiment\"\nlet userId = \"user123\"\nlet forcedVariationKey = \"treatment\"\nlet attributes = [\"device\"           : \"iPhone\",\n                  \"ad_source\"        : \"my_campaign\",\n                  \"$opt_bucketing_id\": \"bucketingId123\"]\n\n// Activate with the bucketing ID\nlet variationKey = try? optimizely.activate(experimentKey: experimentKey,\n                                            userId: userId,\n                                            attributes: attributes)\n\n// Track with the bucketing ID\nlet eventKey = 'myConversion'\ntry? optimizely.track(eventKey: eventKey,\n\t\t\t\t\t\t\t\t\t\t\tuserId: userId, \n                      attributes: attributes)\n\n// Get variation with the bucketing ID\nlet variationKey = try? optimizely.getVariationKey(experimentKey: experimentKey,\n                                                   userId: userId,\n                                                   attributes: attributes)\n                                                   \n                                                   ",
      "language": "swift"
    }
  ]
}
[/block]
Using a bucketing ID does not affect the user ID. Event data submissions will continue to include user IDs. With the exception of assigning users to specific variations, features that rely on user IDs behave the same regardless of the presence of a separate bucketing ID. If you do not pass a bucketing ID to the `attributes` parameter, users are bucketed by user IDs, which is the default method.
[block:callout]
{
  "type": "info",
  "title": "Notes",
  "body": "* The bucketing ID is not persisted.\n* Bucketing IDs are only supported for [feature tests](doc:run-feature-tests) and [A/B tests](doc:run-a-b-tests), not [rollouts](doc:roll-out-and-roll-back-features)."
}
[/block]