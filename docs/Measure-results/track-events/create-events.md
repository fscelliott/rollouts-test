---
title: "Create events"
slug: "create-events"
hidden: false
createdAt: "2019-09-11T11:47:40.640Z"
updatedAt: "2019-09-12T18:58:09.492Z"
---
Use events to track key user behaviors in your Full Stack application or elsewhere in your technology stack. You can track events server-side using one of the Full Stack SDKs, client-side using the JavaScript SDK, or at any other point in your technology stack.

Events can be aggregated over time to produce metrics for your experiments. Create one or more metrics for your experiments to measure the impact.
[block:api-header]
{
  "title": "Create an event"
}
[/block]
To create an event that tracks user actions:
1. Navigate to the _Events_ dashboard and click _New Event_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/d9b3a43-events_1.png",
        "events_1.png",
        1317,
        705,
        "#fafbfc"
      ],
      "border": true
    }
  ]
}
[/block]
2. Define an event key, a unique identifier that you'll reference in your code when you call Track.

Event keys can contain spaces (unlike experiment keys) to conform to industry event-naming standards. Also, event keys must be unique for the project.

In _Event Tracking Code_, Optimizely automatically populates example code for a Track call with your event key and a `user_id`.

3. Click _Save Event_.
[block:image]
{
  "images": [
    {
      "image": [
        "https://files.readme.io/ddc74fb-new_event_1.png",
        "new_event_1.png",
        713,
        634,
        "#f9fafc"
      ],
      "border": true
    }
  ]
}
[/block]