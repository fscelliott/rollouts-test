---
title: "Cross-project events"
slug: "cross-project-events"
hidden: false
createdAt: "2019-09-11T11:47:40.670Z"
updatedAt: "2019-09-12T19:00:16.583Z"
---
Optimizely Full Stack cross-project events help large optimization teams track consistent, program-level metrics across multiple properties.

Usually, an [event ](doc:track-events)lives and is tracked only in the [project](doc:create-projects) where it was created. Cross-project events help you track key events across all Optimizely projects.

With cross-project events, you can:
* Track the same event across different projects to ensure consistency
* Create a set of program-level metrics and reuse them for all your sites
* Define metrics for a global component that crosses into property-specific events
[block:callout]
{
  "type": "info",
  "title": "Note",
  "body": "Cross-project events require the Optimizely 3.0+ SDKs and are available in [Optimizely Enterprise](https://www.optimizely.com/contracts/features/) packages. For more information, contact your account manager."
}
[/block]
Imagine that your company has multiple web properties that are split into separate Optimizely projects. One set of program-level metrics are used to manage all the properties. For example, a key metric tracks engagement with the search bar in the enterprise site and all local store sites. In order to track this same metric in all properties, you'd ordinarily have to set it up in every project. With cross-project events, you can add the pre-defined event to any Optimizely project.

Plus, cross-project events do not affect snippet size! Just enable cross-origin tracking so project snippets can synchronize. And if you expect visitors to cross domains or top-level domains, you can use the [`waitForOriginSync` API](https://developers.optimizely.com/x/solutions/javascript/reference/#function_waitfororiginsync).
[block:api-header]
{
  "title": "Create a cross-project event"
}
[/block]
If you have an [Optimizely Enterprise](https://www.optimizely.com/contracts/features/) account, cross-project events are automatically enabled in your projects. All your events will be cross-project events. [Create an event](doc:create-events) as usual, and it will be available in projects for which you have a [collaborator](doc:invite-collaborators) role.
[block:api-header]
{
  "title": "Add a cross-project metric"
}
[/block]
1. By default, the [Metrics flow](https://help.optimizely.com/Measure_success%3A_Track_visitor_behaviors/Create_a_metric_in_Optimizely_X) shows events in the current project. To include cross-project events, select **All projects**.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/b830218-FS_metrics_all_projects.png",
        "FS metrics all projects.png",
        1185,
        637,
        "#f5f7fb"
      ]
    }
  ]
}
[/block]
2. The *All projects* view shows events that are shared across all Optimizely projects where you’re a collaborator. Click the event to add it as a metric.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/a6896f9-FS_metrics_all_projects_list.png",
        "FS metrics all projects list.png",
        1184,
        764,
        "#f6f8fb"
      ]
    }
  ]
}
[/block]
That’s it! Don’t forget to click **Save** to confirm your changes.
[block:api-header]
{
  "title": "Edit a cross-project event"
}
[/block]
To edit a cross-project event, you'll need to navigate to the project where it was originally created. The project name is listed below the event name.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/3061d4e-FS_project_location.png",
        "FS project location.png",
        1185,
        756,
        "#f6f8fb"
      ]
    }
  ]
}
[/block]
1. Switch to the project where the event lives.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d963fc5-FS_project_selector.png",
        "FS project selector.png",
        415,
        613,
        "#f6f6f9"
      ]
    }
  ]
}
[/block]
2. Under *Events*, select and edit the event.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/2d14f10-FS_events_and_edit.png",
        "FS events and edit.png",
        1086,
        570,
        "#fafafb"
      ]
    }
  ]
}
[/block]