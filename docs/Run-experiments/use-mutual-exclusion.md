---
title: "Use mutual exclusion"
slug: "use-mutual-exclusion"
hidden: false
createdAt: "2019-09-11T11:47:39.191Z"
updatedAt: "2019-09-12T18:49:55.053Z"
---
By default, your experiments in an Full Stack project may overlap with one another, so a single user could be exposed to multiple experiments at the same time. Mutually exclusive experiments help you ensure that a single user can't be exposed to two different experiments running at the same time by ensuring that users are only exposed to one particular experiment.

For example, imagine that you're running two experiments that test the same algorithm. Users who see both experiments could cause interaction effects that taint your experiment results by behaving differently from users who see each experiment in isolation. To clearly understand the impact of each set of changes, make the two experiments mutually exclusive.

In Full Stack projects, you can make two or more experiments mutually exclusive to each other using **exclusion groups**. Using exclusion groups doesn't require any change in the SDKs. The Activate, Is Feature Enabled, and Get Variation methods automatically evaluate whether an experiment is in a group and assign users to the appropriate experiment and variation.
[block:api-header]
{
  "title": "Create an exclusion group"
}
[/block]
To create an exclusion group:

1. Navigate to the _Experiments_ dashboard and select the _Exclusion Groups_ tab.
2. Click _Create an Exclusion Group_.
3. Name and describe the exclusion group.
4. Click _Create Exclusion Group_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/cb1c5ed-exclusion_groups_1.gif",
        "exclusion_groups_1.gif",
        1437,
        763,
        "#fbfbfc"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Add an experiment to an exclusion group"
}
[/block]
To add an experiment to an exclusion group:
1. Click the experiment name.
2. In the _Experiment Traffic Allocation_ section, select the exclusion group from the drop-down menu under _Add this experiment to the following exclusion group_.
3. Add the percentage of the exclusion group traffic that should be allocated to the experiment.
4. Click _Save Experiment_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d369b6c-exclusion_group_2.png",
        "exclusion_group_2.png",
        986,
        548,
        "#f8f8fa"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Remove an experiment from an exclusion group"
}
[/block]
Remove finished experiments from the exclusion group to make space for other experiments.

1. Click the experiment name.
2. In the _Experiment Traffic Allocation_ section, select _None_ from the drop-down menu under _Add this experiment to the following exclusion group_.
3. Click _Save Experiment_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3201853-exclusion_group_3.png",
        "exclusion_group_3.png",
        1020,
        528,
        "#f9f9fb"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Archive and unarchive an exclusion group"
}
[/block]
To archive an exclusion group:
1. Navigate to _Experiments > Exclusion Groups_.
2. Click the  Actions icon (**...**) to the right of the exclusion group you want to archive.
3. Select _Archive_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ff3d706-exclusion_groups_4.png",
        "exclusion_groups_4.png",
        2040,
        562,
        "#f6f7fc"
      ],
      "border": true
    }
  ]
}
[/block]
To find archived exclusion groups and unarchive them:
1. Navigate to _Experiments > Exclusion Groups_.
2. Filter the list to show your archived exclusion groups.
3. Click the  Actions icon (**...**) to the right of the exclusion group you want to unarchive.
4. Select _Unarchive_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a8f19ae-exclusion_group_5.png",
        "exclusion_group_5.png",
        1986,
        728,
        "#f7f9fd"
      ],
      "border": true
    }
  ]
}
[/block]