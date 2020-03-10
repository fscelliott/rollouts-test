---
title: "Manage environments"
slug: "manage-environments"
hidden: false
createdAt: "2019-08-21T19:32:15.837Z"
updatedAt: "2019-08-26T19:32:34.494Z"
---
SYNCED PAGE MODIFICATION TEST FRANCES

Environments allow you to organize a project into sections that correspond to your development environments, like staging and production. With environments, you can turn a feature flag or experiment on in one context, but leave it paused in another. For example, you might turn an experiment on for your team to test in staging, but leave it turned off for users in your production application.

Your account starts with a single environment (and its [datafile](doc:datafile)). When you set up a new environment, Optimizely generates:
 -  **An environment-specific datafile**: The datafile generated for each additional environment contains the current state of all experiments for that specific environment.
 - **A unique SDK key**: The key determines the URL of the environment's datafile and ensures that the URL won’t be easily guessed. For SDKs that provide native datafile managers, you can use the SDK key to initialize the SDK with with the corresponding environment's datafile. See the intialize SDK guide for [Android](doc:initialize-sdk-android) or [Swift](initialize-sdk-swift).

When you create an environment, you'll also create an environment key. This key is used for programmatic access to the environment. For example, you can modify your environments using [Optimizely's REST API](https://developers.optimizely.com/x/rest/introduction/).

Keep separate environments to reduce the risk of accidentally launching an unfinished or unverified test to the live site. You can add as many environments to a project as you need. We recommend adding a staging environment to any project to allow a space for QA.
[block:api-header]
{
  "title": "Create an environment"
}
[/block]
1. Navigate to _Settings > Datafile_.
2. Click _New Environment_.
3. Enter the environment name and key. 
4. Specify whether you'd like to limit the ability to change the active state of a feature flag to [Publishers only](doc:collaborators) in this environment.
5. ![FRANCES LOCAL TEST IMAGE](C:\Users\franc\Documents\Optimizely\TEST_SYNC\subset\images\6f02eed-new_environment.png)
6. Click _Create Environment_. FRANCES: HERE IS A TEST IMAGE: 
[block:image]
{
    "images": [
    {
      "image": [
        "./images/6f02eed-new_environment",
        "new_environment.png",
        738,
        763,
        "#f8f9fb"
      ]
    }
    ]
}
[/block]
The _Environment Key_ and _Datafile URL_ for the new environment will display on the _Settings_ page.
[block:image]
{
    "images": [
    {
      "image": [
        "https://files.readme.io/8c4a98f-environment_key_url.png",
        "environment_key_url.png",
        1371,
        770,
        "#fcfcfe"
      ]
    }
    ]
}
[/block]
6. Implement the datafile in your application.

Learn more about [accessing the datafile](doc:datafile#section-access-the-datafile-via-the-app) and [best practices for datafile management](doc:manage-the-datafile).
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "By default, feature flags are off in any new environments that you create, even if they were previously on or running in another existing environment."
}
[/block]

[block:api-header]
{
  "title": "Rename an environment"
}
[/block]
1. Navigate to _Settings > Datafile_.
2. Click the Actions icon **(...)** for the environment you want to rename.
3. Click _Settings_.
4. Enter the new name for the environment and click _Save Environment_.
[block:image]
{
    "images": [
    {
      "image": [
        "https://files.readme.io/f1988cf-enviroment_rename_1.png",
        "enviroment_rename_1.png",
        1031,
        812,
        "#fbfbfd"
      ]
    }
    ]
}
[/block]

[block:api-header]
{
  "title": "Archive an environment"
}
[/block]
You can archive environments if they aren’t being used anymore (except the primary environment, which can't be archived). Archiving deactivates all the datafiles in an environment, replacing them with an empty file, and removes the environment as an option for features.

If feature flags are on in the environment when you archive it, they will stop because archiving empties the datafile.

To archive an environment:
1. Navigate to _Settings > Datafile_.
2. Click the Actions icon **(...)** for the environment you want to archive.
3. Click _Archive_.
[block:image]
{
    "images": [
    {
      "image": [
        "https://files.readme.io/ba6b7d5-environment_archive_1.png",
        "environment_archive_1.png",
        1025,
        804,
        "#fbfbfd"
      ]
    }
    ]
}
[/block]

[block:api-header]
{
  "title": "Security"
}
[/block]
Keep your production application secure by limiting the permissions of [collaborators](doc:collaborators) who can turn on feature flags in your production environment. 

By default, only collaborators with Publisher-level [permissions](doc:collaborators#section-permissions) make changes to the production environment. You can toggle this permission setting separately in each environment.