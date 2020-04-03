---
title: "Manage experiments"
slug: "manage-experiments"
hidden: false
createdAt: "2019-09-11T11:47:39.176Z"
updatedAt: "2019-11-19T21:40:44.550Z"
---
Use the _Experiments_ dashboard in Full Stack to pause, archive, and change experiments.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b3eef5e-exp_dash_1.png",
        "exp_dash_1.png",
        1586,
        364,
        "#fbfbfd"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Pause an experiment"
}
[/block]
To pause a running experiment, click the Actions icon (**...**). Choose _Pause_ from the dropdown menu for the environment you want to pause. After pausing an experiment, all users will start to receive the original experience.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/279e9aa-exp_dash_2.png",
        "exp_dash_2.png",
        1587,
        479,
        "#fbfbfd"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Pause a variation"
}
[/block]
Once you've started an experiment, you cannot delete a variation. However, you can pause variations instead. When you pause a variation, traffic from that variation will be redistributed among the experiment's other variations, but the results will still be accessible for the paused variation.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/512e8bf-pause-var.png",
        "pause-var.png",
        785,
        451,
        "#faf9f9"
      ],
      "border": true
    }
  ]
}
[/block]
Clicking _Confirm_ alerts Optimizely to stop sending traffic to Var_1. However, the variation can be un-paused later by clicking _Resume_. 

All previous results are still available for any resumed variations.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/9aebb61-resume-var.png",
        "resume-var.png",
        794,
        351,
        "#fbfbfb"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "User profiles and sticky bucketing"
}
[/block]
Pausing a variation is only necessary if you are using Optimizely's user profiles feature. User profiles allow Optimizely users to ensure variation assignments are sticky in any SDK.

If you are working with user profiles and want to ensure that a variation no longer receives any new traffic, you have two options:
 - Pause the variation, which ensures that it will no longer receive any traffic.
 - Set variation traffic distribution percentage to zero, to ensure the variation will no longer receive new traffic, while users who have previously been exposed to the experiment will remain in their assigned variation.

If you haven't implemented user profiles in the SDK, pausing a variation is no different than setting that variation's traffic distribution percentage to zero. 
[block:api-header]
{
  "title": "Archive and un-archive an experiment"
}
[/block]
Archive an experiment to remove it from the _Experiments_ dashboard. Optimizely keeps your experiment data, so you can unarchive the experiment later if you wish.

To archive an experiment, click the  Actions icon (**...**) for the experiment and choose _Archive_ from the dropdown menu.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/6e573a7-exclusion_group_6.png",
        "exclusion_group_6.png",
        1187,
        563,
        "#fbfbfd"
      ],
      "border": true
    }
  ]
}
[/block]
To unarchive an experiment:
1. Click the filter menu and choose _Archived_ from the dropdown.
2. Click the  Actions icon (**...**) for the experiment.
3. Choose _Unarchive_ from the dropdown menu.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/75d5925-exclusion_group_7.png",
        "exclusion_group_7.png",
        1174,
        622,
        "#f9f9fb"
      ],
      "border": true
    }
  ]
}
[/block]

[block:api-header]
{
  "title": "Change experiment parameters"
}
[/block]

[block:callout]
{
  "type": "warning",
  "title": "Important",
  "body": "Be careful when you’re changing experiment parameters, especially the experiment key and variation keys for a running experiment.\n\nDon’t change the experiment key or variation keys unless you’re making the corresponding changes in your code. If you use a key that isn’t referenced in your code, no traffic will be sent to that experiment or variation."
}
[/block]
You can change the experiment parameters, such as traffic allocation, variations, audiences, and events directly in the Experiments dashboard.

1. Click the experiment name to open it.
2. Change your experiment parameters as desired.
3. Click _Save Experiment_.

Changing your experiments will update your project’s datafile within a few seconds. It may take some time before your experiments are updated in production, depending on how often you retrieve datafile updates.

If you want your experiments to update in real-time, use [webhooks](doc:configure-webhooks) to receive datafile updates. 
[block:image]
{
  "images": [
    {
      "image": []
    }
  ]
}
[/block]