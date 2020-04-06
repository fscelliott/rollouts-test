---
title: "Run experiments"
slug: "run-experiments-objective-c"
hidden: false
createdAt: "2019-09-12T15:59:24.084Z"
updatedAt: "2019-09-13T12:09:41.845Z"
---
Once you have [installed](doc:install-sdk-objective-c), [initialized](doc:initialize-sdk-objective-c), and customized the Objective-C SDK for your implementation, you can run experiments and use our feature management and rollout functionality.

This topic is an addendum for specific 3.x topics in this guide by providing Objective-C SDK code samples and file links. Each section includes a link to the applicable 3.x topic for general information. **Make sure that you are using to use the correct 3.1.0-beta code examples and resources when developing your implementation.**
[block:api-header]
{
  "title": "Use feature flags"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "// Evaluate a feature flag and a variable\nbool enabled = [client isFeatureEnabled:@\"price_filter\" userId: userId];\nint min_price = [[client getFeatureVariableInteger:@\"price_filter\" userId: userId] integerValue];\n\n",
      "language": "objectivec"
    },
    {
      "code": "// Evaluate a feature flag and a variable\nlet enabled = optimizelyClient?.isFeatureEnabled(\"price_filter\", userId: userId)\nlet min_price = optimizelyClient?.getFeatureVariableInteger(\"price_filter\", \"min_price\", userId: userId)?.intValue\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Define feature variables"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "- (DiscountAudience *)getDiscountAudience:(OPTLYManager *)manager {\n    DiscountAudience *discountAudience = [[DiscountAudience alloc] init:@\"general\"];\n    if ([manager getOptimizely]) {\n        NSString *domain = @\"\";\n        \n        NSRange range = [self.userId rangeOfString:@\"@\"];\n        if ( range.length > 0 ) {\n            domain = [self.userId substringFromIndex:(range.location + range.length)];\n        }\n        NSString * audienceType = [[manager getOptimizely] getFeatureVariableString:@\"discountAudienceFeatureKey\" variableKey:@\"discountAudienceFeatureVariableKey\" userId:self.userId attributes:@{@\"domainAttr\": domain}];\n        \n        discountAudience = [[DiscountAudience alloc] init:audienceType];\n    }\n    \n    return discountAudience;\n}\n\n",
      "language": "objectivec"
    },
    {
      "code": "// Look for getFeatureVariableString() in the following code.\n\n// Sample usage\nfunc discountAudience() -> DiscountAudience {\n    var audience = DiscountAudience.general\n    guard let userId = UserDefaults.user?.email,\n        let _optimizelyClient = self.optimizelyClient\n        else { return audience }\n\n    let domain = userId.components(separatedBy: \"@\").last ?? \"\"\n    let audienceType = _optimizelyClient.getFeatureVariableString(self.discountAudienceFeaturekey,\n                                                          variableKey: self.discountAudienceFeatureVariableKey,\n                                                          userId: userId,\n                                                          attributes: [domainAttributeKey: domain])\n    audience = DiscountAudience.init(fromString: audienceType ?? \"\")\n    return audience\n}\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Run A/B tests"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "#import \"OPTLYVariation.h\"\n\n// Activate an A/B test\nOPTLYVariation *variation = [client activate:@\"app_redesign\" userId:userId];\nif ([variation.variationKey isEqualToString:@\"control\"]) {\n    // Execute code for \"control\" variation\n} else if ([variation.variationKey isEqualToString:@\"treatment\"]) {\n    // Execute code for \"treatment\" variation\n} else {\n    // Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "objectivec"
    },
    {
      "code": "// Activate an A/B test\nlet variation = client?.activate(\"app_redesign\", userId:\"12122\")\nif (variation?.variationKey == \"control\") {\n// Execute code for \"control\" variation\n} else if (variation?.variationKey == \"treatment\") {\n// Execute code for \"treatment\" variation\n} else {\n// Execute code for users who don't qualify for the experiment\n}\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Assign variations with bucketing IDs"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "NSString *experimentKey = @\"myExperiment\";\nNSString *userId = @\"user123\";\nNSDictionary *attributes = @{@\"device\"              : @\"iPhone\",\n                             @\"ad_source\"           : @\"my_campaign\",\n                             @\"$opt_bucketing_id\"   : @\"bucketingId123\"};\n\n// Activate with the bucketing ID\nOPTLYVariation *variation = [optimizelyClient activate:experimentKey\n                                                userId:user123\n                                            attributes:attributes];\n\n// Track with the bucketing ID\nNSString *eventKey = @\"myEvent\";\n[optimizelyClient track:eventKey\n                        userId:userId\n                    attributes:attributes];\n\n// Get variation with the bucketing ID\nOPTLYVariation *variation = [optimizelyClient activate:experimentKey\n                                                userId:user123\n                                            attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let experimentKey = \"myExperiment\"\nlet userId = \"user123\"\nlet forcedVariationKey = \"treatment\"\nlet attributes = [\"device\"           : \"iPhone\",\n                  \"ad_source\"        : \"my_campaign\",\n                  \"$opt_bucketing_id\": \"bucketingId123\"]\n\n// Activate with the bucketing ID\nlet variation = optimizelyClient?.activate(experimentKey, userId, attributes)\n\n// Track with the bucketing ID\nlet eventKey = 'myConversion'\noptimizelyClient?.track(eventKey, userId, attributes)\n\n// Get variation with the bucketing ID\nvariation = optimizelyClient?.getVariation(experimentKey, userId, attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Use forced bucketing"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "#import \"OptimizelySDKiOS.h\"\n\nNSString *experimentKey = @\"my_experiment\";\nNSString *userId = @\"user123\";\nNSString *forcedVariationKey = @\"treatment\";\n\n// Set a forced variation\n[optimizelyClient setForcedVariation:experimentKey userId:userId variationKey:forcedVariationKey];\n\n// Get a forced variation\nOPTLYVariation *variation = [optimizelyClient getForcedVariation:experimentKey userId:userId];\n\n// Clear a forced variation - set it to null!\n[optimizelyClient setForcedVariation:experimentKey userId:userId variationKey:nil];\n\n",
      "language": "objectivec"
    },
    {
      "code": "import OptimizelySDKiOS\n\nlet experimentKey = \"my_experiment\"\nlet userId = \"user123\"\nlet forcedVariationKey = \"treatment\"\n\n// Set a forced variation\noptimizelyClient?.setForcedVariation(experimentKey, userId: userId, variationKey: forcedVariationKey)\n\n// Get a forced variation\nlet variation = optimizelyClient?.getForcedVariation(experimentKey, userId: userId)\n\n// Clear a forced variation - set it to null!\noptimizelyClient?.setForcedVariation(experimentKey, userId: userId, variationKey: nil)\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Track events"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "// Track a conversion event for the provided user with attributes\n[optimizely track:@\"my_conversion\"\n           userId:@\"user123\"\n       attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "// Track a conversion event for the provided user with attributes\noptimizely?.track(\"my_conversion\", userId:\"user123\", attributes:attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Include event tags"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *attributes = @{@\"device\" : @\"iPhone\", @\"ad_source\" : @\"my_campaign\"};\n\nNSDictionary *eventTags = @{\n  @\"purchasePrice\" : @64.32,\n  @\"category\" : @\"shoes\",\n  @\"revenue\": @6432  // reserved \"revenue\" tag\n};\n\n// Track event with user attributes and event tags\n[optimizely track:@\"my_conversion\"\n           userId:@\"user123\"\n       attributes:attributes\n       eventTags:eventTags];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\"device\" : \"iPhone\", \"ad_source\" : \"my_campaign\"]\n\nvar eventTags = Dictionary<String, Any>()\neventTags[\"purchasePrice\"] = 64.32\neventTags[\"category\"] = \"shoes\"\neventTags[\"revenue\"] = 6432  // reserved \"revenue\" tag\neventTags[\"value\"] = 4 // reserved \"value\" tag\n\n// Track event with user attributes and event tags\noptimizely?.track(\"my_conversion\",\n                  userId:\"user123\",\n                  attributes:attributes,\n                  eventTags:eventTags)\n                  \n                  ",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Manage bot filtering"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "/**\n * Get the user agent and pass it to the Optimizely client \n *  as the attribute $opt_user_agent\n */\n\nNSString *experimentKey = @\"some_experiment\";\nNSString *userId = @\"some_user\";\nNSDictionary *attributes = @{@\"device\"              : @\"iphone\",\n                             @\"location\"            : @\"Chicago\",\n                             @\"$opt_user_agent\"     : @\"this_could_be_a_bot\"};\n\n// Include user agent when activating an A/B test\nOPTLYVariation *variation = [optimizelyClient activate:experimentKey\n                                                userId:userId\n                                            attributes:attributes];\n\n// Include user agent when tracking an event\nNSString *eventKey = @\"some_conversion_event\";\n[optimizelyClient track:eventKey\n                        userId:userId\n                    attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "/**\n * Get the user agent and pass it to the Optimizely client \n *  as the attribute $opt_user_agent\n */\n\nlet experimentKey = \"some_experiment\"\nlet userId = \"some_user\"\nlet attributes = [\"device\"          : \"iphone\",\n                  \"location\"        : \"Chicago\",\n                  \"$opt_user_agent\" : \"this_could_be_a_bot\"]\n\n// Include user agent when activating an A/B test\noptimizelyClient?.activate(experimentKey, userId, attributes)\n\n// Include user agent when tracking an event\nlet eventKey = 'some_conversion_event'\noptimizelyClient?.track(eventKey, userId, attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Access test and variation identifiers"
}
[/block]
For Objective-C, keys and IDs can be accessed on `OPTLYExperiment` objects using `experimentKey` and `experimentId` respectively. Likewise, keys and IDs can be accessed on `OPTLYVariation` objects using `variationKey` and `variationId`.

For Swift, keys and IDs can be accessed on `OPTLYExperiment` objects using `experimentKey` and `experimentId` respectively. Likewise, keys and IDs can be accessed on `OPTLYVariation` objects using `variationKey` and `variationId`.
[block:api-header]
{
  "title": "Register notification listeners"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "// Add an Activate notification listener\nNSInteger activateNotificationId = [optimizely.notificationCenter addDecisionNotificationListener:^(NSString * _Nonnull type, NSString * _Nonnull userId, NSDictionary<NSString *,id> * _Nullable attributes, NSDictionary<NSString *,id> * _Nonnull decisionInfo) {\n    [logger logMessage:@\"decision event\" withLevel:OptimizelyLogLevelDebug];\n}];\n\n// Remove a sepcific listener\n[optimizely.notificationCenter removeNotificationListener:activateNotificationId];\n\n// Remove all of one type of listener\n[optimizely.notificationCenter clearNotificationListeners:OPTLYNotificationTypeDecision];\n\n// Remove all notification listeners\n[optimizely.notificationCenter clearAllNotificationListeners];\n\n",
      "language": "objectivec"
    },
    {
      "code": "// Add an activate notification listener\nlet activateNotificationId = optimizely?.notificationCenter?.addDecisionNotificationListener({ (type, userId, attributes, decisionInfo) in\n    logger?.logMessage(\"decision event\", with: OptimizelyLogLevel.debug)\n})\n\n// Remove a specific notification listener\noptimizely?.notificationCenter?.removeNotificationListener(UInt(activateNotificationId!))\n\n// Remove notification listeners of a certain type\noptimizely?.notificationCenter?.clearNotificationListeners(OPTLYNotificationType.decision)\n\n// Remove all notification listeners\noptimizely?.notificationCenter?.clearAllNotificationListeners()\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Set up Amplitude"
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "#import \"Amplitude.h\"\n#import \"AMPIdentify.h\"\n\n[optimizely.notificationCenter addActivateNotificationListener:^(OPTLYExperiment *experiment, NSString *userId, NSDictionary<NSString *,NSString *> *attributes, OPTLYVariation *variation, NSDictionary<NSString *,NSString *> *event) {\n  NSString *propertyKey\n    = [NSString stringWithFormat:@\"[Optimizely] %@\",\n       experiment.experimentKey];\n  AMPIdentify *identify\n    = [[AMPIdentify identify] set:propertyKey\n       value:variation.variationKey];\n  [[Amplitude instance] identify:identify];\n\n  // Track impression event (optional)\n  NSString *eventIdentifier\n    = [NSString stringWithFormat:@\"[Optimizely] %@ - %@\",\n       experiment.experimentKey,\n       variation.variationKey];\n  [[Amplitude instance] logEvent:eventIdentifier];\n}];\n\n",
      "language": "objectivec"
    },
    {
      "code": "import Amplitude_iOS\nlet activateNotificationId = optimizely?.notificationCenter?.addActivateNotificationListener({ (experiment, userId, attributes, variation, logEvent) in\n  // Set \"user property\" for the user\n  let propertyKey : String! = \"[Optimizely] \" + experiment.experimentKey\n  let identify : AMPIdentify = AMPIdentify()\n  identify.set(propertyKey, value:variation.variationKey as NSObject!)\n\n  // Track impression event (optional)\n  let eventIdentifier : String = \"[Optimizely] \"\n  + experiment.experimentKey + \" - \" + variation.variationKey\n  Amplitude.instance().logEvent(eventIdentifier)\n})\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Set up Google Analytics"
}
[/block]
See general topic: [Set up Google Analytics](doc:set-up-google-analytics). 

If you're getting started, read the Google instructions for your platform:
* [Google Analytics iOS Objective-C Setup instructions](https://developers.google.com/analytics/devguides/collection/ios/v3/)
* [Google Analytics iOS Swift Setup instructions](https://developers.google.com/analytics/devguides/collection/ios/v3/?ver=swift)
[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "The [Google Analytics iOS SDK](https://developers.google.com/analytics/devguides/collection/ios/v3/events) does not support [non-interaction events](https://support.google.com/analytics/answer/1033068#NonInteractionEvents) at this time. This may affect your bounce rate."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "#import <Google/Analytics.h>\n\nNSInteger activateNotificationId = [optimizely.notificationCenter addActivateNotificationListener:^(OPTLYExperiment *experiment, NSString *userId, NSDictionary<NSString *,NSString *> *attributes, OPTLYVariation *variation, NSDictionary<NSString *,NSString *> *event) {\n    // Google Analytics tracker\n    id<GAITracker> tracker = [GAI sharedInstance].defaultTracker;\n\n    NSString *action\n        = [NSString stringWithFormat:@\"Experiment - %@\",\n            experiment.experimentKey];\n    NSString *label\n        = [NSString stringWithFormat:@\"Variation - %@\",\n            variation.variationKey];\n    [tracker send:[[GAIDictionaryBuilder\n        createEventWithCategory:@\"Optimizely\"\n                         action:action\n                          label:label\n                          value:nil] build]];\n}];\n\n",
      "language": "objectivec"
    },
    {
      "code": "// Add an activate notification listener\nlet activateNotificationId = optimizely?.notificationCenter?.addActivateNotificationListener({ (experiment, userId, attributes, variation, logEvent) in\n    // Google Analytics tracker\n    let tracker : GAITracker? = GAI.sharedInstance().defaultTracker\n\n    let action : String = \"Experiment - \" + experiment.experimentKey\n    let label : String = \"Variation - \" + variation.variationKey\n\n    // Build and send an Event\n    let builder = GAIDictionaryBuilder.createEvent(\n        withCategory: \"Optimizely\",\n        action: action,\n        label: label,\n        value: nil).build()\n    tracker?.send(builder as [NSObject : AnyObject]!)\n})\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Set up Localytics"
}
[/block]
See general topic: [Set up Localytics](doc:set-up-localytics). 

### Objective-C

The example code has two parts:
- Add a track event listener to wrap `[Localytics tagEvent]`
- Add `[optimizely track]` to track conversions

Instead of calling `[Localytics tagEvent]` directly, wrap the calls with `[optimizely track]` to include bucketing information as event attributes.

The example code demonstrates how to add the track notification listener. Each `[optimizely track]` event tracking adds a mapping of experiment key to variation key to the event attributes and passes the mapping to `[Localytics tagEvent]`.

The last step is to add `[optimizely track]` to track event conversions.

#### Consistent user identity

Maintaining a consistent user identity across multiple sessions and devices can help ensure proper reporting. Localytics provides some [guidelines](https://docs.localytics.com/dev/ios.html#identifying-users-ios) for their platform.

Optimizely recommends using the same user ID with these methods:
- `[optimizely activate]`
- `[Localytics setCustomerId]`

#### Alternative solution

Another solution is to set Localytics' [Custom Dimensions](https://docs.localytics.com/dashboard/analytics.html#dimensions-custom-dimensions) using an activate notification listener. Custom dimensions can be used to segment users without needing to wrap `[Localytics tagEvent]`, but they require configuration in the Localytics dashboard for each Optimizely test.
[block:code]
{
  "codes": [
    {
      "code": "@import Localytics;\n\n// Add a Track notification listener\nNSInteger trackNotificationId = [optimizely.notificationCenter addTrackNotificationListener:^(NSString * _Nonnull eventKey, NSString * _Nonnull userId, NSDictionary<NSString *,NSString *> * _Nonnull attributes, NSDictionary * _Nonnull eventTags, NSDictionary<NSString *,NSObject *> * _Nonnull event) {\n              // Tag custom event with attributes\n              NSString *eventIdentifier\n                  = [NSString stringWithFormat:@\"[Optimizely] %@\",\n                      eventKey]];\n              [Localytics tagEvent:eventIdentifier attributes:attributes];\n          }];\n\n// Track a conversion event for the provided user\n[optimizely track:eventKey userId:userId];\n\n",
      "language": "objectivec"
    }
  ]
}
[/block]
### Swift

The example code has two parts:
- Add a track notification listener to wrap `Localytics.tagEvent()`
- Add `optimizely.track()` to track conversions

Instead of calling `Localytics.tagEvent()` directly, wrap the calls with `optimizely.track()` to include bucketing information as event attributes.

The example code demonstrates how to add a track notification listener. Each time `optimizely.track()` event tracking adds a mapping of experiment key to variation key to the event attributes and pass the mapping to `Localytics.tagEvent()`.

The last step is to add `optimizely.track()` to track event conversions.

#### Consistent user identity

Maintaining a consistent user identity across multiple sessions and devices can help ensure proper reporting. Localytics provides some [guidelines](https://docs.localytics.com/dev/ios.html#identifying-users-ios) for their platform.

Optimizely recommends using the same user ID with these methods:
- `optimizely.activate()`
- `Localytics.setCustomerId()`

#### Alternative solution

Another solution is to set Localytics' [Custom Dimensions](https://docs.localytics.com/dashboard/analytics.html#dimensions-custom-dimensions) using an activate notification listener. Custom dimensions can be used to segment users without needing to wrap `Localytics.tagEvent()` but requires configuration in the Localytics dashboard for each Optimizely test.
[block:code]
{
  "codes": [
    {
      "code": "import Localytics\n\n// Add a track notification listener\noptimizely?.notificationCenter?.addTrackNotificationListener({ (eventKey, userId, attributes, eventTags, event) in\n  // Tag custom event with attributes\n  let localyticsEventIdentifier : String = \"[Optimizely] \" + eventKey\n  Localytics.tagEvent(localyticsEventIdentifier, attributes)\n})\n\noptimizely?.track(eventKey, userId)\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Set up Mixpanel"
}
[/block]
Code example for [Set up Mixpanel](doc:set-up-mixpanel). 
[block:code]
{
  "codes": [
    {
      "code": "#import \"Mixpanel/Mixpanel.h\"\n\n[optimizely.notificationCenter addActivateNotificationListener:^(OPTLYExperiment *experiment, NSString *userId, NSDictionary<NSString *,NSString *> *attributes, OPTLYVariation *variation, NSDictionary<NSString *,NSString *> *event) {\n  // Mixpanel instance\n  Mixpanel *mixpanel = [Mixpanel sharedInstance];\n\n  NSString *propertyKey\n    = [NSString stringWithFormat:@\"[Optimizely] %@\",\n       experiment.experimentKey];\n  [mixpanel\n   registerSuperProperties:@{propertyKey: variation.variationKey}];\n\n  // Set \"People property\" for the user\n  [mixpanel.people set:@{propertyKey: variation.variationKey}];\n\n  // Track impression event (optional)\n  NSString *eventIdentifier\n    = [NSString stringWithFormat:@\"[Optimizely] %@ - %@\",\n       experiment.experimentKey,\n       variation.variationKey];\n  [mixpanel track:eventIdentifier];\n}];\n\n",
      "language": "objectivec"
    },
    {
      "code": "import Mixpanel\n\noptimizely?.notificationCenter?.addActivateNotificationListener({ (experiment, userId, attributes, variation, logEvent) in\n  // Mixpanel instance\n  let mixpanel : MixpanelInstance = Mixpanel.mainInstance()\n\n  // \"Super property\" will be sent with all future track calls\n  let propertyKey : String! = \"[Optimizely] \" + experiment.experimentKey\n  mixpanel.registerSuperProperties([propertyKey: variation.variationKey])\n\n  // Set \"People property\" for the user\n  mixpanel.people.set(property: propertyKey, to: variation.variationKey)\n\n  // Track impression event (optional)\n  let eventIdentifier : String = \"[Optimizely] \"\n  + experiment.experimentKey + \" - \" + variation.variationKey\n  mixpanel.track(event:eventIdentifier)\n})\n\n",
      "language": "swift"
    }
  ]
}
[/block]
Next in the callback, we can optionally log an impression event that signifies that the Optimizely test was activated for the current user. You can use this event or another event you may already be tracking to calculate a conversion rate. 

We recommend using the same user ID with the following methods:
[block:parameters]
{
  "data": {
    "h-0": "Language",
    "h-1": "Activate Callback",
    "0-1": "- `[optimizely activate]`\n- `[mixpanel createAlias]`\n- `[mixpanel identify]`",
    "1-1": "- `optimizely.activate()`\n- `mixpanel.createAlias()`\n- `mixpanel.identify()`",
    "0-0": "Objective-C",
    "1-0": "Swift"
  },
  "cols": 2,
  "rows": 2
}
[/block]
#### Compare results

When comparing Optimizely and Mixpanel results, remember to apply a date filter in Mixpanel to correspond with the dates your Optimizely test was running. People properties and super properties will remain set in Mixpanel even after your Optimizely test has stopped running.

#### Consistent user identity

Maintaining a consistent user identity across multiple sessions and devices can help ensure proper reporting. Mixpanel provides some [guidelines](https://help.mixpanel.com/hc/en-us/articles/115004497803) for their platform.
[block:api-header]
{
  "title": "Set up mParticle"
}
[/block]
See the general topic: [Set up mParticle](doc:set-up-mparticle). 

If you’re getting started, read the [mParticle Getting Started guide for the iOS SDK](https://docs.mparticle.com/developers/sdk/ios/getting-started/).

### Step 1: Prerequisites
  * Make sure that the required experiment is running correctly in Optimizely.
  * Install the mParticle SDK with the additional [Optimizely kit](https://docs.mparticle.com/integrations/optimizely/event/#adding-the-kit-to-your-ios-or-android-app) on the iOS app.

### Step 2: Enable the integration

The example code below sends a DECISION notification listener that contains information about the user, experiment, and variation to mParticle in an event called `Experiment Viewed`. 
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "You can customize the event name and also specify which event tags are included the event payload."
}
[/block]

[block:code]
{
  "codes": [
    {
      "code": "@import mParticle_Apple_SDK;\n \n// Conditionally activate an experiment for the provided user, can use mParticle ids \nOPTLYVariation *variation = [optimizely activate:@\"my_experiment\" \n                                          userId:[MParticle sharedInstance].identity.deviceApplicationStamp];\n \n// mParticle event\nMPProduct *product = [[MPProduct alloc] initWithName:@\"Foo name\"\n                                                 sku:@\"Foo sku\"\n                                            quantity:@4\n                                               price:@100.00];\nMPTransactionAttributes *attributes = [[MPTransactionAttributes alloc] init];\nattributes.transactionId = @\"foo-transaction-id\";\n// mapped to Optimizely as 45000 cents\nattributes.revenue = @450.00;\nattributes.tax = @30.00;\nattributes.shipping = @30;\n \nMPCommerceEventAction action = MPCommerceEventActionPurchase;\nMPCommerceEvent *event = [[MPCommerceEvent alloc] initWithAction:action\n                                                         product:product];\n \n// mapped to Optimizely as a custom event name\n[event addCustomFlag:@\"custom revenue event name\"\n             withKey:MPKitOptimizelyEventName];\nevent.transactionAttributes = attributes;\n[[MParticle sharedInstance] logCommerceEvent:event];\n \n \n// DECISION notification listener that is bound to activate() and sends an event to mParticle with the experiment and variation information\n \n[optimizely.notificationCenter addDecisionNotificationListener:^(NSString *type, NSString *userId, NSDictionary<NSString *,id> *attributes, NSDictionary<NSString *,id> *decisionInfo) {\n     \n\tMPEvent *event = [[MPEvent alloc] initWithName:@\"Experiment Viewed\"\n                                          type:MPEventTypeOther];\n\t NSDictionary *eventInfo = @{\n           @\"experimentName\" : decisionInfo[@\"experimentKey\"],\n           @\"variationName\" : decisionInfo[@\"variationKey\"]\n           };\n      [[MParticle sharedInstance] logEvent:event eventInfo:eventInfo];\n \n}];\n\n",
      "language": "objectivec"
    },
    {
      "code": "import mParticle_Apple_SDK\n \n// Optimizely DECISION notification listener for activate() \nlet activateNotificationId = optimizely?.notificationCenter?.addDecisionNotificationListener { (type, userId, attributes, decisionInfo) in\n        let eventInfo = [\"experimentName\": decisionInfo[\"experimentKey\"] as Any,\n                  \"variationName\": decisionInfo[\"variationKey\"] as Any]\n    \n\t   MParticle.sharedInstance().logEvent(\"Experiment Viewed\", eventType: MPEventType.other, eventInfo: eventInfo)\n \n}\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Set up Segment"
}
[/block]
Segment has a semantic event that you can use to [track A/B test variations for users](https://segment.com/docs/spec/ab-testing/).

In the example code below, we add an activate listener block callback to send Segment's A/B event.
[block:code]
{
  "codes": [
    {
      "code": "#import <Analytics/SEGAnalytics.h>\n[optimizely.notificationCenter addActivateNotificationListener:^(OPTLYExperiment *experiment, NSString *userId, NSDictionary<NSString *,NSString *> *attributes, OPTLYVariation *variation, NSDictionary<NSString *,NSString *> *event) {\n       NSDictionary *properties = @{\n           @\"experimentId\" : [experiment experimentId],\n           @\"experimentName\" : [experiment experimentKey],\n           @\"variationId\" : [variation variationId],\n           @\"variationName\" : [variation variationKey]\n           };\n       [[SEGAnalytics sharedAnalytics] track:@\"Experiment Viewed\" properties:properties];\n}];\n\n",
      "language": "objectivec"
    },
    {
      "code": "import Analytics\n\nlet activateNotificationId = optimizely?.notificationCenter?.addActivateNotificationListener({ (experiment, userId, attributes, variation, logEvent) in\n        let properties = [\"experimentId\": experiment.experimentId as Any,\n                  \"experimentName\": experiment.experimentKey as Any,\n                  \"variationId\": variation.variationId as Any,\n                  \"variationName\": variation.variationKey as Any]\n    \n        SEGAnalytics.shared().track(\"Experiment Viewed\", properties: properties)\n})\n\n",
      "language": "swift"
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "API reference updates"
}
[/block]
This section provides updated Objective-C SDK code examples for all Optimizely methods.

Source files containing the implementations are at [Optimizely.h](https://github.com/optimizely/objective-c-sdk/blob/master/OptimizelySDKCore/OptimizelySDKCore/Optimizely.h).

### Instantiate

#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "datafile\nerrorHandler\neventDispatcher\nlogger\nuserProfileService\nclientVersion\nclientEngine\n\n",
      "language": "objectivec"
    },
    {
      "code": "datafile\nerrorHandler\neventDispatcher\nlogger\nuserProfileService\nclientVersion\nclientEngine\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Code example
[block:code]
{
  "codes": [
    {
      "code": "let optimizelyClient: OPTLYClient? = optimizelyManager?.initialize(withDatafile:jsonDatafile!)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Activate

#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nNSString *variationKey = [optimizely activateWithExperimentKey: @\"my_experiment_key\"\n                          userId:@\"user_123\"\n                          attributes:attributes\n                          error:nil];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet variationKey = try? optimizely.activate(experimentKey:\"my_experiment_key\",\n                                            userId: \"user_123\",\n                                            attributes: attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Returns
[block:code]
{
  "codes": [
    {
      "code": "@return The key of the variation where the user is bucketed, or `nil` if the user doesn't qualify for the experiment.\n\n",
      "language": "objectivec"
    },
    {
      "code": "@return The key of the variation where the user is bucketed, or `nil` if the user doesn't qualify for the experiment.\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Code example
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nOPTLYVariation *variation = [optimizelyClient activate:@\"my_experiment_key\",\n                                                userId:@\"user_123\",\n                                            attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet variation = optimizelyClient?.activate(\"my_experiment_key\", userId:\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Get Enabled Features

#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "userId\nattributes\n\n",
      "language": "objectivec"
    },
    {
      "code": "userId\nattributes\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Returns
[block:code]
{
  "codes": [
    {
      "code": "@return A list of keys corresponding to the features that are enabled for the user, or an empty list if no features could be found for the specified user.\n\n",
      "language": "objectivec"
    },
    {
      "code": "@return NSArray<NSString> A list of keys corresponding to the features that are enabled for the user, or an empty list if no features could be found for the specified user.\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Code example
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nNSArray<NSString *> *enabledFeatures = [optimizelyClient getEnabledFeatures:@\"user_123\"\n                                                                 attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet enabledFeatures = optimizelyClient?.getEnabledFeatures(\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Get Feature Variable

#### Boolean

Returns the value of the specified Boolean variable.
[block:code]
{
  "codes": [
    {
      "code": "- (NSNumber *)getFeatureVariableBoolean:(nullable NSString *)featureKey\n                      variableKey:(nullable NSString *)variableKey\n                           userId:(nullable NSString *)userId\n                       attributes:(nullable NSDictionary<NSString *, NSString *> *)attributes;\n\n",
      "language": "objectivec"
    },
    {
      "code": "- (NSNumber *)getFeatureVariableBoolean:(nullable NSString *)featureKey\n                      variableKey:(nullable NSString *)variableKey\n                           userId:(nullable NSString *)userId\n                       attributes:(nullable NSDictionary<NSString *, NSString *> *)attributes\n                       \n                       ",
      "language": "swift"
    }
  ]
}
[/block]
#### Double

Returns the value of the specified double variable.
[block:code]
{
  "codes": [
    {
      "code": "- (NSNumber *)getFeatureVariableDouble:(nullable NSString *)featureKey\n                       variableKey:(nullable NSString *)variableKey\n                            userId:(nullable NSString *)userId\n                        attributes:(nullable NSDictionary<NSString *, NSString *> *)attributes;\n\n",
      "language": "objectivec"
    },
    {
      "code": "- (NSNumber *)getFeatureVariableDouble:(nullable NSString *)featureKey\n                       variableKey:(nullable NSString *)variableKey\n                            userId:(nullable NSString *)userId\n                        attributes:(nullable NSDictionary<NSString *, NSString *> *)attributes;\n                        \n                        ",
      "language": "swift"
    }
  ]
}
[/block]
#### Integer

Returns the value of the specified integer variable. 
[block:code]
{
  "codes": [
    {
      "code": "- (NSNumber *)getFeatureVariableInteger:(nullable NSString *)featureKey\n                     variableKey:(nullable NSString *)variableKey\n                          userId:(nullable NSString *)userId\n                      attributes:(nullable NSDictionary<NSString *, NSString *> *)attributes;\n\n",
      "language": "objectivec"
    },
    {
      "code": "- (NSNumber *)getFeatureVariableInteger:(nullable NSString *)featureKey\n                     variableKey:(nullable NSString *)variableKey\n                          userId:(nullable NSString *)userId\n                      attributes:(nullable NSDictionary<NSString *, NSString *> *)attributes;\n                      \n                      ",
      "language": "swift"
    }
  ]
}
[/block]
#### String

Returns the value of the specified string variable.
[block:code]
{
  "codes": [
    {
      "code": "- (NSString *_Nullable)getFeatureVariableString:(nullable NSString *)featureKey\n                           variableKey:(nullable NSString *)variableKey\n                                userId:(nullable NSString *)userId\n                            attributes:(nullable NSDictionary<NSString *, NSString *> *)attributes;\n\n",
      "language": "objectivec"
    },
    {
      "code": "- (NSString *_Nullable)getFeatureVariableString:(nullable NSString *)featureKey\n                           variableKey:(nullable NSString *)variableKey\n                                userId:(nullable NSString *)userId\n                            attributes:(nullable NSDictionary<NSString *, NSString *> *)attributes;\n                            \n                            ",
      "language": "swift"
    }
  ]
}
[/block]
#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "featureKey\nvariableKey\nuserId\nattributes\n\n",
      "language": "objectivec"
    },
    {
      "code": "featureKey\nvariableKey\nuserId\nattributes\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Returns
[block:code]
{
  "codes": [
    {
      "code": "@return The value of the variable, or `nil` if the feature key is invalid, the variable key is invalid, or there is a mismatch with the type of the variable.\n\n",
      "language": "objectivec"
    },
    {
      "code": "experimentKey\nuserId\nattributes\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Code example
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nNSNumber *featureVariableValue = [optimizelyClient getFeatureVariableDouble:@\"my_feature_key\",\n                                                                variableKey:@\"double_variable_key\",\n                                                                     userId:@\"user_123\",\n                                                                 attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet featureVariableValue = optimizelyClient?.getFeatureVariableDouble(\"my_feature_key\", variableKey:\"double_variable_key\", userId:\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Get Forced Variation

#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "experimentKey\nuserId\n\n",
      "language": "objectivec"
    },
    {
      "code": "experimentKey\nuserId\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Returns
[block:code]
{
  "codes": [
    {
      "code": "@return The variation the user was bucketed into, or `null` if `setForcedVariation` failed to force the user into the variation.\n\n",
      "language": "objectivec"
    },
    {
      "code": "@return forced variation if it exists, otherwise return nil.\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Code example
[block:code]
{
  "codes": [
    {
      "code": "let variation = optimizelyClient?.getForcedVariation(”my_experiment_key”, userId:”user_123”);\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet variation = optimizelyClient?.activate(\"my_experiment_key\", userId:\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Get Variation

#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "experimentKey\nuserId\nattributes\n\n",
      "language": "objectivec"
    },
    {
      "code": "experimentKey\nuserId\nattributes\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Returns
[block:code]
{
  "codes": [
    {
      "code": "@return The variation into which the user is bucketed. This value can be nil.\n\n",
      "language": "objectivec"
    },
    {
      "code": "@return The variation into which the user was bucketed. This value can be nil.\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Code example
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nOPTLYVariation *variation = [optimizelyClient variation:@\"my_experiment_key\",\n                                                 userId:@\"user_123\",\n                                             attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet variation = optimizelyClient?.variation(\"my_experiment_key\", userId:\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Is Feature Enabled

#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "featureKey\nuserId\nattributes\n\n",
      "language": "objectivec"
    },
    {
      "code": "featureKey\nuserId\nattributes\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Returns
[block:code]
{
  "codes": [
    {
      "code": "@return YES if feature is enabled, false otherwise.\n\n",
      "language": "objectivec"
    },
    {
      "code": "@return YES if feature is enabled, false otherwise.\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Code example
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nbool enabled = [optimizelyClient isFeatureEnabled:@\"my_feature_key\",\n                                           userId:@\"user_123\",\n                                       attributes:attributes];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet enabled = optimizelyClient?.isFeatureEnabled(\"my_feature_key\", userId:\"user_123\", attributes:attributes)\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Set Forced Variation

#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "experimentKey\nuserId\nvariationKey\n\n",
      "language": "objectivec"
    },
    {
      "code": "experiment_key\nuser_id\nvariation_key\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Returns
[block:code]
{
  "codes": [
    {
      "code": "@return `YES` if the user was successfully forced into a variation, `NO` if the `experimentKey` isn't in the project file or the `variationKey` isn't in the experiment.\n\n",
      "language": "objectivec"
    },
    {
      "code": "@return A boolean value that indicates if the set completed successfully.\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Code example
[block:code]
{
  "codes": [
    {
      "code": "[optimizelyClient setForcedVariation:@\"my_experiment_key\"\n                              userId:@\"user_123\"\n                        variationKey:@\"some_variation_key\"];\n\n",
      "language": "objectivec"
    },
    {
      "code": "optimizelyClient?.setForcedVariation(”my_experiment_key”, userId:”user_123”, variationKey:\"some_variation_key\");\n\n",
      "language": "swift"
    }
  ]
}
[/block]
### Track

#### Parameter names
[block:code]
{
  "codes": [
    {
      "code": "eventKey\nuserId\nattributes\neventTags\n\n",
      "language": "objectivec"
    },
    {
      "code": "eventKey\nuserId\nattributes\neventTags\n\n",
      "language": "swift"
    }
  ]
}
[/block]
#### Returns

This method sends conversion data to Optimizely. It doesn't provide return values. 

#### Code example
[block:code]
{
  "codes": [
    {
      "code": "NSDictionary *attributes = @{\n  @\"device\": @\"iPhone\",\n  @\"lifetime\": @24738388,\n  @\"is_logged_in\": @true\n};\n\nNSDictionary *tags = @{\n  @\"category\" : @\"shoes\",\n  @\"count\": @5\n};\n\n[optimizelyClient track:@\"my_event_key\"\n                 userId:@\"user_123\"\n             attributes:attributes\n              eventTags:tags];\n\n",
      "language": "objectivec"
    },
    {
      "code": "let attributes = [\n  \"device\": \"iPhone\",\n  \"lifetime\": 24738388,\n  \"is_logged_in\": true,\n]\n\nlet tags = [\n  \"category\": \"shoes\",\n  \"count\": 2,\n]\n\noptimizelyClient?.track(\"my_event_key\", userId:\"user_123\", attributes:attributes, eventTags:tags);\n\n",
      "language": "swift"
    }
  ]
}
[/block]