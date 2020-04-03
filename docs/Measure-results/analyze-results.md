---
title: "Analyze results"
slug: "analyze-results"
hidden: false
createdAt: "2019-09-11T11:47:39.236Z"
updatedAt: "2019-12-04T18:10:33.751Z"
---
The Results page is powered by Optimizely's Stats Engine, a [unique approach to statistics for the digital age](http://pages.optimizely.com/rs/optimizely/images/stats_engine_technical_paper.pdf) developed in conjunction with leading researchers at Stanford University. Stats Engine embeds innovations combining sequential testing and false discovery rate control to deliver [speed and accuracy for businesses](https://blog.optimizely.com/2015/01/20/statistics-for-the-internet-age-the-story-behind-optimizelys-new-stats-engine/) making decisions based on real-time data. 

Once you run a feature test or standalone A/B test, dig into the [Results page](https://help.optimizely.com/Analyze_Results/The_Experiment_Results_page_for_Optimizely_X) to learn how users respond to your experiment.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/4e5762e-results-graph.gif",
        "results-graph.gif",
        1364,
        762,
        "#f9f9fa"
      ],
      "caption": "",
      "border": true
    }
  ]
}
[/block]
For more information, see these Optimizely Knowledge Base articles:
 -  [Stats Accelerator](https://help.optimizely.com/Build_Campaigns_and_Experiments/Stats_Accelerator)
 - [How Optimizely calculates results](https://help.optimizely.com/Analyze_Results/Stats_Engine%3A_How_Optimizely_calculates_results_to_enable_business_decisions).

Full Stack does not currently support retroactive results calculation.
[block:callout]
{
  "type": "info",
  "body": "Feature rollouts help you launch new features using a feature flag. When you use rollouts, Optimizely [doesn't send an impression](doc:interactions-between-feature-tests-and-rollouts), so no extra network traffic is generated and no results are attached. To measure the impact of a feature, run a feature test instead.",
  "title": "Note"
}
[/block]

[block:api-header]
{
  "title": "Segment results"
}
[/block]
Segment your results to see how different groups of users behave, compared to users overall. 

By default, Optimizely shows results for all users who enter your experiment. However, not all users behave like your average users. Optimizely lets you filter your results so you can see if certain groups of users behave differently from your users overall. This is called segmentation.

For example, imagine you run an experiment with a pop-up promotional offer. This generates positive lift overall, but when you segment for users on mobile devices, it's a statistically significant loss. Maybe the pop-up is disruptive or difficult to close on a mobile device. When you implement the change or run a similar experiment in the future, you might exclude mobile users based on what you've learned from segmenting.

Segmenting results is one of the best ways to gain deeper insight beyond the average user's behavior. It's a powerful way to step up your experimentation program.

Experiments in Full Stack projects don’t include “out-of-the-box” attributes like browser, device, or location attributes in Web. Optimizely’s SDKs are platform-agnostic: we don't assume which attributes are available in your application or what the format is. In Full Stack, all segmentation is based on the custom attributes that you create.

Here's how to set them up: 

1. [Create the custom attributes](doc:define-attributes) that you want to use for Results page segmentation.
2. [Create audiences](doc:define-audiences-and-attributes) based on your custom attributes.
3. Pass the custom attributes into the Activate, Track, and Get Variation functions for your experiment or app. For an example, see [Pass in attributes for segmenting results](doc:define-audiences-and-attributes#section-pass-in-attributes).

After you define custom attributes and pass them in the SDK, they'll be available as segmentation options from the drop-down menu at the top of the Results page.

[block:api-header]
{
  "title": "Interpret the Results page"
}
[/block]
You can segment your entire Results page or the results for an individual metric. Segmenting results helps you get more out of your data by generating valuable insights about your user.

1. Navigate to your [Results page](https://help.optimizely.com/Analyze_Results/The_Experiment_Results_page_for_Optimizely_X).
2. Click *Segment* and select an attribute from the dropdown.

Under the *Segment* dropdown, you'll find default segments and custom segments all in one place.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/28d76eb-segments.png",
        "segments.png",
        1566,
        609,
        "#f9f9fa"
      ],
      "border": true
    }
  ]
}
[/block]

[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "For Optimizely to use attributes for segmentation, the attribute must be defined in the datafile, and it must be included in both the Activate and Track calls. However, it does not have to be added as an audience to the test."
}
[/block]
When segmenting results, a user who belongs to more than one segment will be counted in every segment they belong to. However, if a user has more than one value for a single segment, the user is only counted for the last-seen value they had in the session. 

See our KB article on [how visitors and conversions are counted in segments](https://help.optimizely.com/Analyze_Results/How_Optimizely_counts_conversions#Filtering_by_segment).
[block:api-header]
{
  "title": "See also"
}
[/block]
 - [Discrepancies in third-party data](https://help.optimizely.com/Analyze_Results/Discrepancies_in_third-party_data)
 - [How long to run an experiment](https://help.optimizely.com/Analyze_Results/How_long_to_run_an_experiment)
 - [How Optimizely counts conversions](https://help.optimizely.com/Analyze_Results/How_Optimizely_counts_conversions) 
 - [Interpret your results](https://help.optimizely.com/Analyze_Results/Interpret_your_results)
 - [Stats Accelerator: Use algorithms to boost results](https://help.optimizely.com/Build_Campaigns_and_Experiments/Stats_Accelerator)
 - [Take action based on the results of an experiment](https://help.optimizely.com/Analyze_Results/Take_action_based_on_the_results_of_an_experiment)
 - [Tracking offline conversion events with Optimizely Classic and Optimizely X](https://help.optimizely.com/Measure_success%3A_Track_visitor_behaviors/Tracking_offline_conversion_events_with_Optimizely_Classic_and_Optimizely_X)